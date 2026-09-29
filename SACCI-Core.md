# Especificações Técnicas: SACCI-Core (Planejamento Central)

O `SACCI-Core` é o núcleo do [SACCI](SACCI.md): recebe a contabilidade dos nós, mantém a matriz insumo-produto, calcula e recomputa o plano, os custos sociais e o fechamento, verifica os dados, e distribui ordens e informação. Este documento é a especificação de implementação; os algoritmos estão em [Algoritmos](Algoritmos.md), as regras de governança em [Governança e Privacidade](Governanca%20e%20Privacidade.md). A escolha de linguagens e ferramentas é livre, sujeita aos requisitos abaixo.

## 1. Componentes

| Componente | Função |
|---|---|
| **Ingestão** | Recebe payloads assinados dos nós e pontos de distribuição; valida assinatura, encadeamento e schema; enfileira |
| **Livro (ledger)** | Armazena a contabilidade dos nós: lançamentos imutáveis, encadeados por nó; casa as duas pontas de cada `Transfer` e cada `ServiceRecord` |
| **Matriz** | Mantém A, l, o grafo produtivo, a lista de nós críticos; atualiza coeficientes observados; verifica ρ(A) |
| **Planejador** | Balanço material, horas embutidas, LP com função objetivo votada; produz versões do plano e valores duais |
| **Custos** | Custo de plano, normalização do FAA, custo de equilíbrio, razão P/P*, fechamento e d |
| **Verificação** | Detecção de anomalias; indicadores de vigilância; canal de contestação e auditorias |
| **Distribuição** | Decompõe o plano em `ProductionOrder`/`SupplyOrder`; gera `ChangeRequest`; publica explicações |
| **Simulação** | Executa a simulação de opções de voto e cenários de choque sobre cópias do estado |
| **API pública** | Consulta da contabilidade dos nós, do plano, dos custos, das anomalias, das explicações |
| **Comparador** | Recebe os resultados das implementações independentes, compara, publica divergências |

## 2. Modelo de dados

Todas as entidades são públicas, exceto onde indicado. Nenhuma entidade contém dados individuais.

- **Node**: `Id`, `PublicKey`, `Type` (coletiva | cooperativa | ponto de distribuição), `CellId`, `SphereIds[]`, `Critical` (bool), `SpecialRegimeSince`.
- **Item**: `Code`, `Name`, `Unit`, `ConsumptionGood` (bool), `ImpactClass` (para FAA), `MinimumDomain` (Mínimos, opcional).
- **Coefficient**: `OutputItem`, `InputItem`, `NodeId`, `Declared`, `Observed`, `Period` — de onde A é agregado (média ponderada pela produção, por item).
- **DirectHours**: `Item`, `NodeId`, `HoursPerUnit`, `Period` — de onde l é agregado.
- **LedgerEntry**: `NodeId`, `Seq`, `PreviousHash`, `Type` (Production | TransferOut | TransferIn | Service | Capacity | Stock | Hours), `Payload`, `Signature`, `ReceivedAt`.
- **Transfer**: as duas pontas casadas; `Status` (Pending | Confirmed | Discrepant); `Discrepancy`.
- **Plan**: `Id`, `Horizon`, `VotedAt`, `Parameters` (lei de parâmetros), `ObjectiveFunction`, `FinalDemand y`, `Constraints`, `Minimums`.
- **PlanVersion**: `PlanId`, `Version`, `ComputedAt`, `Cause` (voto | replanejamento | choque), `GrossOutput x`, `EmbeddedHours H`, `Duals`, `d`, `Closure` (termos da equação), `Orders[]`, `ImplementationHashes[]`.
- **Order**: `ProductionOrder` / `SupplyOrder` conforme [NodeClient](NodeClient.md) §2, com `Explanation` (cadeia de demanda, restrições ativas, prioridades, duais).
- **ChangeRequest**: conforme NodeClient; `Decision` e `VoteRecord` (contagem apenas).
- **Cost**: `Item`, `Period`, `PlanCost P*`, `EquilibriumCost P`, `Ratio`.
- **Anomaly**: `Type`, `NodeIds[]`, `Evidence`, `OpenedAt`, `Status`, `Resolution`.
- **Contestation** / **Audit**: quem abriu, alvo, prazo, resultado, custo imputado.
- **Parameters**: a lei de parâmetros vigente, versionada ([Parâmetros](Parametros.md)).
- **ImplementationResult**: `ImplementationId`, `PlanVersion`, `ResultHash`, `Summary` — para o comparador.

## 3. APIs

### 3.1 Ingestão (autenticada por chave do nó)
- `POST /api/v1/node/{nodeId}/sync` — payload de [NodeClient](NodeClient.md) §3; resposta com ordens, `ChangeRequest`s, confirmações, alertas, custos.
- `POST /api/v1/node/{nodeId}/transfer/confirm` — confirmação de entrada.
- `POST /api/v1/node/{nodeId}/change-request/{id}/decision` — decisão do conselho.
- `POST /api/v1/node/{nodeId}/contestation` — abertura de contestação.
- `POST /api/v1/distribution/{pointId}/sync` — agregados de consumo por bem/período/região e compromissos ([Distribuição](Distribuicao.md)).

### 3.2 Pública (leitura, sem autenticação)
- `GET /api/v1/node/{id}` — contabilidade do nó; `GET /api/v1/node/{id}/ledger?from=&to=`.
- `GET /api/v1/plan/current`, `GET /api/v1/plan/{id}/version/{v}` — plano, x, H, duais, fechamento.
- `GET /api/v1/order/{id}/explanation`.
- `GET /api/v1/cost/{item}?period=`.
- `GET /api/v1/anomaly?status=&sphere=`.
- `GET /api/v1/graph/critical-nodes`.
- `GET /api/v1/parameters/current`.
- `GET /api/v1/implementations/compare/{planVersion}`.
- `GET /api/v1/simulation/{optionId}` — resultado da simulação de uma opção de voto.

### 3.3 Interna
- Simulação sob demanda do [sistema de votação](VoteSystem.md): `POST /internal/simulate` com uma opção (Δy, ΔA, parâmetros) → `PlanVersion` hipotética.

Todos os payloads têm schema público e versionado; mudanças de schema seguem [Governança e Privacidade](Governanca%20e%20Privacidade.md) §1.1.

## 4. Ciclos de execução

| Ciclo | Frequência de referência | Conteúdo |
|---|---|---|
| **Ingestão** | contínua | validação, ledger, casamento de transferências |
| **Replanejamento operacional** | horário/diário | atualiza x e ordens dentro das janelas e limites; ativa substitutos; gera `ChangeRequest`s quando a alteração excede as janelas |
| **Custos** | por período de apuração (diário) | custo de equilíbrio; razão P/P* |
| **Coeficientes** | semanal | recomputa A e l observados; anomalias de coeficiente e capacidade; ρ(A) |
| **Fechamento** | mensal | d, ΔS, validações do fechamento |
| **Plano** | por votação | nova `PlanVersion` a partir do plano votado e da lei de parâmetros |
| **Choque** | por evento | recomputação imediata (ver [SACCI](SACCI.md) §5) |
| **Comparação** | a cada `PlanVersion` | hashes das implementações independentes; divergência > tolerância bloqueia a distribuição de ordens |

## 5. Requisitos

- **Escala**: milhões de itens, centenas de milhares de nós; A esparsa em memória (formato CSR/CSC); cálculos de §1–§2 de [Algoritmos](Algoritmos.md) em minutos em hardware comum, segundos em cluster.
- **Reprodutibilidade**: toda `PlanVersion` é recomputável a partir do ledger público e dos parâmetros; o hash do resultado é publicado.
- **Determinismo**: mesma entrada, mesmo resultado, em todas as implementações (aritmética com tolerância declarada; ordem de iteração fixada).
- **Disponibilidade**: redundância geográfica; o ledger é replicado; a queda do planejador não interrompe a ingestão nem a execução das ordens vigentes.
- **Segurança**: verificação de assinatura e encadeamento em toda entrada; API pública somente leitura; sem dados individuais em nenhum componente.
- **Auditabilidade**: logs de intervenção operacional do corpo técnico, públicos.

## 6. Implementações independentes

Cada implementação é um serviço completo (§1) mantido por um corpo técnico distinto, com código público e repositório próprio. Todas consomem o mesmo ledger. O comparador publica, por `PlanVersion`, os hashes e um resumo das diferenças (x, H, d, ordens). Divergência acima da tolerância suspende a distribuição de ordens novas — as ordens vigentes continuam — até resolução publicada.

## 7. Simulador de opções e de choques

- Recebe uma opção (variação de y, de A, de parâmetros ou de restrições) e devolve uma `PlanVersion` hipotética com x, H, d, B, nível de atendimento por setor, uso de limites físicos e carga de atenção.
- Recebe um cenário de choque (remoção de nó, variação de demanda, corte de importação) e devolve a trajetória de recomputação (tempo até novo plano, déficits por nó e data, substitutos ativados, `ChangeRequest`s gerados).
- É o mesmo código do planejador, executado sobre uma cópia do estado; os resultados são públicos.
