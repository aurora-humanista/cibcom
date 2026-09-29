# Especificações Técnicas: NodeClient (Nó Produtivo)

O `NodeClient` é o software operacional base do CiberCom, instalado em cada unidade de produção local (uma cooperativa têxtil, uma fazenda, uma fábrica de eletrônicos, etc.). Ele coleta os dados do chão de fábrica, mantém a **contabilidade do nó** (pública) e a **contabilidade das pessoas** (local e privada), e se comunica com o SACCI-Core para o planejamento econômico transparente e racional.

Este documento é a especificação de implementação do nó; a do planejamento central está em [SACCI-Core](SACCI-Core.md). As regras que ele implementa estão em [SACCI](SACCI.md), [Verificação e Incentivos](Verificacao%20e%20Incentivos.md), [Governança e Privacidade](Governanca%20e%20Privacidade.md) e [Tokens de Valor](tokens-valor.md).

---

## 🏗️ 1. Arquitetura do Sistema Local

O sistema segue uma arquitetura de App Desktop **Offline-First**, que roda inteiramente na máquina do nó produtivo (PC industrial, Raspberry Pi, etc.), mantendo comunicação assíncrona com a central do SACCI. Em caso de partição de rede, os lançamentos ficam em fila local e são sincronizados depois, sem perda de ordem.

### Processo principal (backend local)
- **Sync & Mock Workers**: processos secundários para escuta de ordens do SACCI, envio de telemetria, confirmação de recebimentos e o "Simulador de Eventos" (mock produtivo).
- **Chaves**: o nó possui um par de chaves próprio (assinatura de todos os lançamentos). Cada trabalhador possui sua **chave de trabalho**, guardada no seu dispositivo ou cartão; o nó nunca guarda chaves privadas de pessoas.
- **IPC**: comunicação entre a interface e o backend local por canal interno com superfície mínima (a interface nunca acessa o banco diretamente).

### Interface (frontend local)
- Aplicação de página única servida localmente, sem dependência de rede para operar.
- Gerenciamento de estado reativo; componentes para totens de chão de fábrica e para o conselho.

---

## 🗄️ 2. Modelo de Dados (Schema)

O schema separa as duas contabilidades. Tabelas marcadas **[nó]** são sincronizadas com o SACCI e públicas; tabelas marcadas **[pessoas]** nunca saem do nó senão como totais com compromisso criptográfico.

### **Item** [nó]
Tipologia do que entra e do que sai.
- `Id` (GUID)
- `Code` (String): código universal do produto no catálogo do SACCI (ex: "ELETRO-102").
- `Name` (String)
- `Type` (Enum): `Input` | `Output` | `Both`
- `UnitOfMeasurement` (String): kg, litros, unidades, horas.

### **BillOfMaterials** [nó]
A Lista de Materiais (LM) de cada item produzido pelo nó: coeficientes técnicos declarados, que o SACCI compara com os observados e com os de nós semelhantes.
- `Id` (GUID)
- `OutputItemId` (FK -> Item)
- `Components` (List<{ `ItemId`, `QuantityPerUnit` }>)
- `DirectHoursPerUnit` (Decimal)
- `AuthorizedSubstitutes` (List<{ `ItemId`, `SubstituteItemId`, `Ratio`, `Threshold` }>): substitutos técnicos autorizados pelo plano.
- `Version`, `ValidFrom`

### **Stock** [nó]
- `Id` (GUID)
- `ItemId` (FK -> Item)
- `Quantity` (Decimal)
- `LastUpdated` (DateTimeOffset)

### **Capacity** [nó]
Capacidade declarada, por item, comparada pelo SACCI com a produção histórica e com o mínimo técnico da LM.
- `ItemId` (FK -> Item)
- `MaxUnitsPerPeriod` (Decimal)
- `AvailableHoursPerPeriod` (Decimal)
- `DeclaredAt` (DateTimeOffset)

### **Worker** [pessoas]
- `Id` (GUID)
- `WorkKeyFingerprint` (String): impressão da chave de trabalho; o nome real fica cifrado com a chave do trabalhador.
- `Role` (String)
- `Multiplier` (Decimal): mᵢ, **1 por padrão**; só assume outro valor por prêmio tabelado (penosidade, escassez, criticidade), com referência ao parâmetro da lei de parâmetros que o autoriza.

> Não existe "tokens por hora por trabalhador" configurável: a regra é 1 VT por hora, igual para todos; os prêmios são tabelados e auditáveis ([Tokens de Valor](tokens-valor.md), §2).

### **ProductionRecord** [nó, com detalhe de horas em pessoas]
- `Id` (GUID)
- `Timestamp` (DateTimeOffset)
- `InputItems` (List<{ `ItemId`, `Amount` }>)
- `OutputItems` (List<{ `ItemId`, `Amount` }>)
- `TotalHours` (Decimal) — **[nó]**: sincronizado.
- `WorkerHours` (List<{ `WorkerId`, `Hours` }>) — **[pessoas]**: não sincronizado; o nó envia apenas `TotalHours` e um compromisso criptográfico de que a soma das parcelas é igual ao total.
- `Signature`: assinatura do nó; `PreviousHash`: encadeamento dos lançamentos.

### **Transfer** [nó] — partida dobrada
Todo fluxo entre nós tem dois lançamentos independentes.
- `Id` (GUID)
- `OrderId` (FK -> SupplyOrder, opcional)
- `FromNodeId`, `ToNodeId`
- `ItemId`, `Quantity`
- `Direction` (Enum): `Outbound` (este nó entregou) | `Inbound` (este nó recebeu)
- `CounterpartTransferId` (GUID, nulo até a confirmação da outra ponta)
- `Status` (Enum): `Pending` | `Confirmed` | `Discrepant`
- `QualityAssessment` (opcional, no `Inbound`): conformidade e avaliação.

### **ServiceRecord** [nó]
Serviços prestados ou recebidos; a confirmação do destinatário é o segundo lançamento.
- `Id`, `ProviderNodeId`, `RecipientNodeId` (ou ponto de distribuição para consumo final), `Description`, `Hours`, `ConfirmedAt`, `Assessment`.

### **ProductionOrder** [nó] (antes `MacroGoal`)
Ordem recebida do SACCI: decomposição operacional do plano votado.
- `Id` (GUID)
- `OutputItemId` (FK -> Item)
- `TargetQuantity` (Decimal)
- `QualitySpec` (String)
- `DeliveryWindowStart`, `DeliveryWindowEnd` (DateTimeOffset)
- `Priority` (Int)
- `PhysicalLimits` (JSON): tetos de energia, água, materiais, emissões aplicáveis.
- `Explanation` (JSON): cadeia de demandas, restrições e valores duais que produziram a ordem (direito de explicabilidade).
- `Status` (Enum): `Pending` | `InProgress` | `Reached` | `Failed` | `Contested`

### **SupplyOrder** [nó]
Ordem de suprimento a receber (insumos despachados pelo SACCI a este nó) ou a entregar (este nó como fornecedor).
- `Id`, `ItemId`, `Quantity`, `CounterpartNodeId`, `WindowStart`, `WindowEnd`, `Status`.

### **ChangeRequest** [nó]
Proposta do SACCI que exige alteração de intensidade ou de tempo de trabalho: precisa do consentimento do conselho.
- `Id` (GUID)
- `Type` (Enum): `Overtime` | `ExtraShift` | `Anticipation` | `Intensification`
- `AffectedHours` (Decimal)
- `PremiumMultiplier` (Decimal): tabelado, vindo da lei de parâmetros.
- `ResponseDeadline` (DateTimeOffset): T_a; silêncio = recusa.
- `Decision` (Enum): `Pending` | `Accepted` | `Refused` | `Arbitrated` (nós em regime especial)
- `VoteRecord` (JSON): resultado da votação do conselho (contagem, não votos individuais).

### **Experiment** [nó]
Uso do tempo protegido e do orçamento de experimentação ([Inovação e Entrada](Inovacao%20e%20Entrada.md)).
- `Id`, `Description`, `Hours`, `BillOfMaterialsDraft`, `Gate` (Enum: `Prototype` | `Pilot` | `Plan`), `Deadline`, `Status`.

### **Contestation** [nó]
Contestação aberta por este nó contra um fornecedor (auditoria física) ou contra uma ordem.
- `Id`, `TargetNodeId` ou `OrderId`, `Reason`, `OpenedAt`, `Outcome`.

---

## 📡 3. Comunicação Interna (IPC) e Sincronização (Rede)

Toda a regra de negócio e o contato com o banco local ficam no **processo principal**.

### (IPC)
- `get-stock`, `get-capacity`, `get-bom`
- `register-production`: baixa de insumos, alta de produto, horas por trabalhador (assinadas pela chave de trabalho no terminal).
- `register-transfer-out` / `confirm-transfer-in`: as duas pontas da partida dobrada.
- `get-orders`, `get-order-explanation`, `contest-order`
- `get-change-requests`, `vote-change-request`
- `open-contestation`
- `get-experiments`, `register-experiment-hours`

### Sincronização com o SACCI-Core
A cada intervalo configurável, o processo principal envia um payload **assinado** com a contabilidade do nó:

```json
{
  "nodeId": "UUID-DA-FABRICA-42",
  "period": "2026-09-28T10:00Z/2026-09-28T11:00Z",
  "stockLevels": [
    { "itemCode": "MADEIRA-BRUTA", "qty": 1400 },
    { "itemCode": "CADEIRA-PRONTA", "qty": 45 }
  ],
  "capacity": [ { "itemCode": "CADEIRA-PRONTA", "maxUnitsPerPeriod": 60 } ],
  "production": [ { "recordId": "…", "outputs": [...], "inputs": [...], "totalHours": 120.5 } ],
  "transfers": [ { "transferId": "…", "direction": "Inbound", "counterpartNodeId": "…", "itemCode": "MADEIRA-BRUTA", "qty": 400 } ],
  "hoursCommitment": "<compromisso criptográfico: Σ horas por trabalhador = 120.5>",
  "signature": "<assinatura do nó>"
}
```

O que **nunca** vai no payload: horas por trabalhador, nomes, chaves de trabalho, votos individuais do conselho.

A resposta contém: ordens novas ou atualizadas (`ProductionOrder`, `SupplyOrder`), `ChangeRequest`s pendentes, confirmações de partida dobrada da outra ponta, alertas de discrepância e de desvio de coeficientes, e o custo de equilíbrio atual dos produtos do nó (informativo). Transporte: HTTPS com mensageria assíncrona; o nó consulta os dados públicos dos nós de sua cadeia pela API pública do SACCI.

---

## 💻 4. Interface de Usuário

UX pragmática para o conselho de trabalhadores e para terminais de chão de fábrica (totens).

1. **Dashboard** (`/dashboard`): ordens vs. produzido; janelas de entrega; medidores de insumos com alerta antes da ruptura; custo de equilíbrio dos produtos e razão P/P* (informativo).
2. **Terminal de Produção** (`/production-entry`): registro de lote; baixa de insumos pela LM; horas por trabalhador assinadas no terminal.
3. **Fluxos** (`/transfers`): entregas e recebimentos pendentes de confirmação; discrepâncias.
4. **Conselho** (`/council`): `ChangeRequest`s com prazo e prêmio tabelado; votação; contestações; explicação de ordens.
5. **Pessoal** (`/workers`): cadastro de chaves de trabalho, funções e multiplicadores autorizados; extrato local de horas (visível apenas ao próprio trabalhador com sua chave).
6. **Experimentação** (`/experiments`): tempo protegido, propostas e portões.

---

## 🧪 5. Simulador Embarcado (Mock Mode)

Com `MOCK_SIMULATION=true` na configuração, um loop no processo principal:

1. Sorteia um produto da lista `Item` com LM.
2. Verifica os insumos no estoque virtual; se abaixo do limiar, usa o substituto autorizado ou gera um `SupplyOrder`.
3. Consome os recursos, insere um `ProductionRecord` com horas distribuídas entre trabalhadores simulados e gera `Transfer`s `Outbound` para um nó a jusante simulado.
4. Opcionalmente injeta **má-fé configurável** — subdeclaração de capacidade, superdeclaração de insumos, atraso de confirmação — para testar a camada de verificação do SACCI-Core ([Programa de Verificação](Programa%20de%20Verificacao.md), §2).
5. Envia o estado ao SACCI-Core pelo processo de sincronia.

---

## 🔒 6. Requisitos de segurança e privacidade

- Lançamentos assinados e encadeados; o SACCI-Core rejeita lançamentos sem assinatura válida ou fora de ordem.
- Horas por trabalhador cifradas em repouso; extrato individual acessível apenas com a chave de trabalho.
- Nenhum endpoint do NodeClient expõe dados individuais; a API pública do nó expõe apenas a contabilidade do nó.
- Chaves de trabalho, consumo e voto são distintas e não vinculáveis ([Governança e Privacidade](Governanca%20e%20Privacidade.md), §2.3).
