# Next Steps: Roteiro de Desenvolvimento do CiberCom

Este documento é o roteiro prático para tirar o **Sistema Automatizado de Coleta e Computação de Informação (SACCI)** do papel. O projeto exige a conexão entre os **Nós Produtivos** (locais físicos de produção e estoque), o **Planejamento Central** (o motor algorítmico) e o **Sistema de Votação** (a democracia direta digital). Cada fase implementa regras já especificadas na documentação do modelo; os documentos de referência estão indicados.

---

## 🏗️ Fase 1: O Software do Nó Produtivo (`NodeClient`)

Referência: [NodeClient](NodeClient.md), [SACCI](SACCI.md), [Verificação e Incentivos](Verificacao%20e%20Incentivos.md).

O `NodeClient` roda na "ponta" (fábrica, fazenda, cooperativa). Não faz grandes cálculos de planejamento; coleta dados em tempo real, mantém as duas contabilidades e executa as ordens vindas do plano votado.

### Responsabilidades
1. **Registro local**: insumos recebidos, produtos fabricados, horas trabalhadas (totais sincronizados; por trabalhador, locais).
2. **Partida dobrada**: todo fluxo com outro nó gera lançamento de saída e confirmação de entrada.
3. **Telemetria**: estado atual (estoques, capacidade declarada, coeficientes observados) para o SACCI, assinado.
4. **Ordens e consentimento**: receber ordens de produção e suprimento com janelas e explicação; submeter ao conselho as alterações de intensidade/tempo com prêmio tabelado e prazo.

### Etapas
- [ ] Schema conforme [NodeClient](NodeClient.md) §2: `Item`, `BillOfMaterials`, `Stock`, `Capacity`, `Worker` (mᵢ = 1 por padrão), `ProductionRecord` (horas totais vs. por trabalhador), `Transfer` (partida dobrada), `ServiceRecord`, `ProductionOrder`, `SupplyOrder`, `ChangeRequest`, `Experiment`, `Contestation`.
- [ ] Assinatura e encadeamento de lançamentos; compromisso criptográfico das horas.
- [ ] **Mock de Eventos** com má-fé configurável (subdeclaração de capacidade, superdeclaração de insumos, atraso de confirmação).

---

## 🧠 Fase 2: O Software do Planejamento Central (`SACCI-Core`)

Referência: [SACCI-Core](SACCI-Core.md), [Algoritmos](Algoritmos.md), [Planos de Produção](Planos%20de%20Producao.md), [Governança e Privacidade](Governanca%20e%20Privacidade.md).

### Responsabilidades
1. **Ingestão**: receber os payloads assinados de todos os nós (HTTP e mensageria assíncrona); rejeitar lançamentos inválidos; casar as duas pontas de cada `Transfer`.
2. **Matriz e horas embutidas**: manter A e l; calcular x = (I − A)⁻¹y e H = l(I − A)⁻¹ por Gauss–Seidel/Jacobi esparso.
3. **Otimização**: programação linear com função objetivo votada e limites físicos; publicar valores duais.
4. **Custos sociais**: custo de plano P* = FAA·H com FAA normalizado; custo de equilíbrio P com ganho e limite de variação; razão P/P*.
5. **Fechamento**: calcular d (ou Q/B) pela equação (1 − d)·Σ mᵢhᵢ + B·N − ΔS = Σ FAAⱼHⱼQⱼ.
6. **Verificação**: discrepâncias de partida dobrada; dispersão de coeficientes entre nós comparáveis; capacidade declarada vs. histórico; grafo produtivo e pontos de articulação (nós críticos).
7. **Distribuição**: decompor o plano em `ProductionOrder`/`SupplyOrder` com janelas, prioridades, substitutos e explicação; gerar `ChangeRequest`s com prêmio tabelado e prazo.

### Etapas
- [ ] API de ingestão (`POST /api/node/sync`) e API pública de consulta (`GET /api/node/{id}`, `GET /api/plan/current`, `GET /api/order/{id}/explanation`).
- [ ] Algoritmo v1: matriz 10×10 fixa → Gauss–Seidel → horas embutidas → custos de plano → fechamento.
- [ ] Algoritmo v2: LP com limites físicos e função objetivo parametrizada; valores duais.
- [ ] Módulo de verificação (partida dobrada, coeficientes, capacidade, grafo).
- [ ] **Duas implementações independentes** do núcleo, por equipes distintas, com comparação automática dos resultados (k = 2 no desenvolvimento; k = 3 em produção).

---

## 🗳️ Fase 3: O Sistema de Votação (`vote-system`)

Referência: [VoteSystem](VoteSystem.md), [Democracia Direta Digital](Democracia%20Direta%20Digital.md) §6–8, [Quórum Mínimo](Quorum%20Minimo.md), [Algoritmos](Algoritmos.md) §6–7.

### Responsabilidades
1. Cédulas com **preferência ordenada**; apuração por Condorcet + participação q + adesão a; "mais próxima do consenso" por Copeland ponderado.
2. **Lei de parâmetros** como pacote único com o plano; **decisão por exceção** com prazo e fração de contestação; **delegação por tema** com teto e votos públicos dos delegados; **minipúblicos** sorteados.
3. **Verificabilidade de ponta a ponta sem recibo**, revoto até o fechamento (vale o último), alternativa em papel consolidada.
4. **Chave de voto** distinta e não vinculável às chaves de trabalho e consumo.
5. Publicação das simulações de cada opção (executadas por todas as implementações do SACCI-Core) e das recomendações dos minipúblicos.

### Etapas
- [ ] Escolher e auditar um protocolo de votação verificável de código aberto (critérios: E2E-V, sem recibo, revoto).
- [ ] Implementar apuração (Condorcet/Copeland) com testes contra casos conhecidos.
- [ ] Implementar delegação por tema, teto D_max, decisão por exceção e teto de atenção V_max.
- [ ] Sorteio auditável para minipúblicos e para a fila de experimentação.

---

## 🌐 Fase 4: Integração, Simulação e Validação em Rede

Referência: [Programa de Verificação](Programa%20de%20Verificacao.md).

### Etapas
- [ ] **Ambiente de simulação orquestrado em contêineres**: 1–2 instâncias do `SACCI-Core` (implementações independentes) + armazenamento + N instâncias do `NodeClient` (ex.: 50) com Mock de Eventos.
- [ ] **Cisne negro**: desligar arbitrariamente um nó crítico; medir o tempo da queda até o novo plano; verificar ativação de substitutos e `ChangeRequest`s.
- [ ] **Má-fé**: ativar subdeclaração/superdeclaração em uma fração dos nós; medir taxa de detecção e evolução da folga declarada.
- [ ] **Dados reais**: carregar a MIP 2015 do IBGE (e uma base multirregional) como matriz A; calcular H, P*, d implícito; verificar o fechamento com B e ΔS plausíveis; reproduzir o resultado de Dapprich (2023) sobre FAA.
- [ ] **Carga decisória**: simulação de participação com custos de atenção heterogêneos (decisão por exceção, delegação, minipúblicos).
- [ ] Publicar resultados e parâmetros usados; registrar contra os **critérios de refutação**.

---

## 🚀 Fase 5: O Caminho para Produção

- Autenticação e criptografia de ponta a ponta (nós → core; chaves de trabalho, consumo e voto).
- Interface para conselhos e corpos deliberativos: planos, simulações, valores duais, anomalias, contestações.
- Ponto de distribuição conforme [Distribuição](Distribuicao.md): destruição de VT, entrega dos Mínimos, agregação por bem/período/região com granularidade mínima, poupança finalista com prazos, pré-compromisso.
- Otimização do núcleo (implementação de baixo nível e/ou GPU) para escala de milhões de produtos.
- **Experimento em escala reduzida** com uma rede de cooperativas reais, dentro da legalidade vigente, medindo registro, contestação, participação e troca informal.
