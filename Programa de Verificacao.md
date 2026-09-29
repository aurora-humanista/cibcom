# Programa de Verificação

As regras do CiberCom não se demonstram; testam-se. Este documento diz o que será testado, com quais dados, e o que contaria como refutação. Uma proposta que nenhum resultado pode contrariar não é uma proposta. As três frentes abaixo podem ser iniciadas hoje, sem esperar por nenhuma transição.

## 1. Simulação com matrizes de insumo-produto reais

**Dados.** Matriz de Insumo-Produto do Brasil 2015 (IBGE, Contas Nacionais n. 62, 2018) e bases multirregionais públicas (EXIOBASE, WIOD, OECD ICIO) com resolução setorial semelhante; dados de horas trabalhadas por setor (PNAD Contínua, Contas Nacionais); dados de emissões e uso de água por setor.

**O que calcular.**
1. Horas embutidas H por setor/produto e custos de plano P*.
2. A dedução d implícita na estrutura atual: fração das horas destinada a investimento, administração, serviços públicos e transferências. Verificar se a equação de fechamento (1 − d)·Σ mᵢhᵢ + B·N − ΔS = Σ FAAⱼHⱼQⱼ fecha com B e ΔS de magnitudes plausíveis.
3. FAA normalizado por classe de impacto (emissões, água) e seu efeito sobre custos relativos; reproduzir com dados brasileiros o resultado de Dapprich (2023) de que a valoração por custo de oportunidade dá mais peso a objetivos ambientais que o valor-trabalho puro.
4. Choques: remoção de um nó (o incêndio da fábrica de borracha), choque de demanda, choque de oferta importada; resposta pela regra P/P* com amortecimento; medir tempo de convergência e amplitude de oscilação.
5. Restrições de mínimos: custo em horas de garantir os Mínimos Cibercomunistas por domínio, e sua compatibilidade com o d resultante.

**Limitação.** A resolução setorial de uma matriz nacional é grosseira em relação a um SACCI real; basta para testar a coerência macro do fechamento e a estabilidade da dinâmica.

## 2. Simulação baseada em agentes

**Regime de incentivos.** Centenas a milhares de nós, cada um com uma estratégia de reporte (honesto, subdeclaração de capacidade, superdeclaração de insumos, recusa estratégica), submetidos às regras de [Verificação e Incentivos](Verificacao%20e%20Incentivos.md): partida dobrada, comparação de coeficientes, prêmios tabelados, prazos, redundância, metas por comparação, regime especial. Medir: folga declarada agregada ao longo do tempo; taxa de detecção; convergência do plano sob ruído e má-fé realistas; ponto em que a folga deixa de compensar.

**Carga decisória.** População simulada com custos de atenção heterogêneos, submetida às regras de [Democracia Direta Digital](Democracia%20Direta%20Digital.md) (lei de parâmetros, decisão por exceção, delegação por tema com teto, minipúblicos, teto de atenção). Medir: participação efetiva versus quórum; concentração de delegações; frequência de dissoluções; captura por minorias mobilizadas.

**Esteira de entrada.** Propostas com qualidade heterogênea entrando pelos portões de [Inovação e Entrada](Inovacao%20e%20Entrada.md). Medir: taxa de entrada de bens novos, taxa de saída automática, uso do orçamento de experimentação, viés do sorteio ponderado.

## 3. Experimento em escala reduzida

Uma rede de cooperativas reais operando com uma implementação do SACCI e vales internos, dentro da legalidade vigente, como já fazem redes de economia solidária com moedas sociais. É o único lugar onde se testa o que nenhuma simulação alcança: se as pessoas registram, contestam, delegam e sorteiam como o desenho supõe. Medir: qualidade dos registros, uso dos canais de contestação, participação nas decisões, tamanho da troca informal.

## 4. Critérios de refutação

| Resultado | O que refuta |
|---|---|
| A equação de fechamento só fecha com um d que a população plausivelmente não aceitaria, ou com B abaixo de qualquer piso decente | O desenho distributivo |
| A dinâmica P/P* oscila em vez de convergir sob choques da magnitude dos observados em cadeias reais | O mecanismo de custo de equilíbrio precisa de amortecimento não previsto |
| A folga declarada cresce com o tempo apesar das regras de incentivo | A resposta a Kornai é insuficiente |
| A participação cai abaixo do quórum em poucos meses no experimento, ou as delegações se concentram em poucos | A carga decisória está subestimada |
| Os Mínimos não cabem no d resultante sem sacrificar o consumo individual a níveis inaceitáveis | O piso material é incompatível com a estrutura produtiva atual e exige plano de longo prazo |
| A troca informal no experimento supera uma fração relevante do consumo | O custo de equilíbrio não está lendo preferências |

Cada uma dessas falhas é observável. É assim que "coerente" se converte em "viável" — ou não.

## 5. Organização

- Repositório de simulação público, com os dados, o código e os resultados reproduzíveis.
- Resultados publicados por frente, com os parâmetros usados ([Parâmetros](Parametros.md)).
- Revisão aberta: qualquer pessoa pode reproduzir, contestar e propor cenários.
