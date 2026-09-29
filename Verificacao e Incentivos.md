# Verificação e Incentivos

Este documento trata do problema que derrubou o planejamento do século XX e que nenhum computador resolve sozinho: a veracidade dos dados e a barganha sobre metas. A leitura de referência é a de Kornai (*Economics of Shortage*, 1980): a economia de escassez decorre da restrição orçamentária mole e do "efeito catraca" — cada empresa subdeclarava capacidade, superdeclarava necessidades e negociava metas folgadas, porque o cumprimento de hoje virava a meta de amanhã. O Gosplan tinha os relatórios; não tinha como distingui-los da verdade.

O CiberCom responde em três camadas: o que o SACCI verifica **por construção**; o que exige **regra explícita**; e o que fica com **atores com interesse direto**, institucionalmente protegidos.

## 1. O que o SACCI verifica por construção

| Grandeza | Mecanismo | Observação |
|---|---|---|
| **Fluxos entre nós** | Partida dobrada: a saída registrada pelo emissor e a entrada registrada pelo receptor são lançamentos independentes; a discrepância aparece sozinha | Cobre insumos, produtos intermediários, energia, serviços intermediários (o nó receptor confirma) |
| **Consumo final** | Registrado no ato pela destruição de VT; não é estimado | Cobre bens e serviços de consumo individual |
| **Serviços** | A destruição do VT pelo destinatário é a confirmação de que o serviço existiu; para serviços intermediários, o nó receptor confirma | Uma consulta ou um conserto só existem para o SACCI quando quem recebeu registrou |
| **Coeficientes técnicos** | Comparação entre nós que produzem o mesmo bem: um nó que declara 1,2 kg de farinha por kg de pão quando os demais usam 1,0 é imediatamente visível | A remuneração uniforme por hora torna a ineficiência imputável à organização do trabalho, não a salários baixos |
| **Qualidade** | Confirmação do destinatário com avaliação; registros de devolução e garantia; razão P/P* dos serviços e bens de consumo | Sinal, não medida direta |

Dado público não é dado verificado: publicar muda quem pode olhar, não o que se pode ver. O que escapa aos mecanismos acima são as grandezas **sem contraparte**: a **capacidade declarada**, as **horas registradas** e a **qualidade interna** a um nó.

## 2. Auditores com interesse direto

"O público" não audita milhões de itens. Os auditores reais são dois, e o modelo os institucionaliza:

- **Nós a jusante.** Um nó prejudicado pela folga de um fornecedor pode abrir um **canal de contestação**: exigir auditoria física (estoques, máquinas, capacidade) do fornecedor, executada por um corpo técnico da esfera. O custo da auditoria é imputado a quem estiver errado. A auditoria examina a contabilidade do nó, nunca dados individuais.
- **Trabalhadores de dentro.** Quem está no conselho sabe a verdade. O **denunciante** que reporta divergência entre o registro e o fato tem proteção explícita: anonimato perante o nó, garantia de posto e de renda, e prioridade de realocação se preferir sair. A denúncia dispara auditoria física.

Capacidade e horas, sem contraparte, são verificadas por esses dois canais e pela comparação com a produção histórica e com o mínimo técnico da Lista de Materiais.

## 3. Barganha sobre metas

O efeito catraca não precisa de dado falso; precisa de poder de retenção sobre a meta. Um conselho que pode recusar uma alteração e negociar seu preço detém, se produz um insumo crítico, poder sobre toda a cadeia. O modelo mantém o consentimento sobre o trabalho e retira dele o caráter de barganha, por quatro regras da lei de parâmetros:

1. **Prêmios tabelados.** O prêmio por alteração de intensidade ou de tempo de trabalho é fixado por tipo de alteração (hora extra, turno adicional, antecipação, intensificação), como os demais multiplicadores, e **não é negociado caso a caso**.
2. **Prazo de resposta.** O conselho tem T_a (referência: 48 h para choques; 10 dias para replanejamento ordinário) para aceitar ou recusar. Silêncio conta como recusa; a recusa devolve a demanda ao SACCI para redistribuição a outros nós.
3. **Redundância mínima de fornecedores.** O plano de médio prazo mantém, para cada insumo crítico, pelo menos r fornecedores (referência: r = 2) ou capacidade substituta autorizada, precisamente para que nenhum conselho seja indispensável.
4. **Metas por comparação, não por histórico.** A meta do período seguinte de um nó é derivada da comparação com nós semelhantes e do mínimo técnico da LM, não do desempenho passado do próprio nó. Isso remove o incentivo a produzir abaixo da capacidade para proteger a meta futura.

## 4. Regime especial de nós críticos

Para o **produtor único** — um porto, uma hidrelétrica, a única fábrica de um componente — não há redundância imediata nem par com que comparar, e o poder de retenção é máximo. O SACCI identifica esses nós por construção: são os **pontos de articulação do grafo produtivo**, cuja remoção desconecta cadeias. A lista é pública. Para eles:

- O conselho conserva a autonomia sobre a execução (sequenciamento, turnos, mix).
- O direito de recusar alterações é substituído por **arbitragem vinculante** da esfera afetada, em prazo fixo, com o prêmio limitado pela tabela.
- Os trabalhadores do nó recebem um **prêmio permanente de criticidade** (multiplicador tabelado).
- A comparação é feita contra o **mínimo técnico** da LM e contra referências de engenharia.
- O plano de médio prazo assume o compromisso de **construir redundância** e o nó sai do regime quando ela existir.
- O regime é de exceção: lista pública, prazo, revisão a cada plano, saída automática.

## 5. Incentivo ao trabalho e à unidade

- **Individual**: a renda é proporcional às horas e ao tipo de trabalho ([Tokens de Valor](tokens-valor.md)). A hora registrada não mede intensidade nem qualidade; o que responde ao esforço individual é a autogestão — o conselho tem a informação e o interesse para distribuir tarefas e reconhecer contribuições. O modelo aposta na pressão dos pares e na ausência de um patrão a quem enganar, não em vigilância do trabalhador.
- **Da unidade**: a remuneração uniforme torna as diferenças de produtividade entre unidades visíveis e imputáveis à organização do trabalho. Um nó com coeficientes piores que os comparáveis recebe do SACCI as práticas dos melhores (quadros técnicos por tecnologia, ver [Inovação e Entrada](Inovacao%20e%20Entrada.md)) e um prazo para convergir; a reincidência leva à revisão do direito de uso ([Cooperativas](Cooperativas.md)).

## 6. Indicadores de vigilância do sistema

O SACCI publica, por plano, os indicadores que diriam se a resposta a Kornai está falhando:

- Folga declarada agregada (capacidade declarada − produção) por setor, e sua tendência.
- Taxa de recusa de alterações por nó e por setor; tempo médio de resposta.
- Discrepâncias de partida dobrada por período (número e volume).
- Dispersão de coeficientes técnicos entre nós comparáveis.
- Número de nós em regime especial e prazo médio de permanência.
- Contestações abertas, auditorias realizadas e resultado.

Uma folga declarada que cresce apesar destas regras é o sinal de que elas são insuficientes ([Programa de Verificação](Programa%20de%20Verificacao.md)).
