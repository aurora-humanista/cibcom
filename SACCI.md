# Sistema Automatizado de Coleta e Computação de Informação (SACCI)

O SACCI é o sistema nervoso da economia planejada: coleta dados de produção, insumos, mão de obra e consumo em tempo real; mantém a matriz insumo-produto da economia; decompõe o plano votado em ordens; recomputa o plano diante de choques; e devolve a informação a todos os nós produtivos, aos corpos deliberativos e à população. Ele substitui a sinalização de preços entre unidades produtivas por um fluxo de informação em unidades físicas e em tempo de trabalho.

Este documento trata da arquitetura e da operação. A especificação de implementação do núcleo está em [SACCI-Core](SACCI-Core.md), a do nó em [NodeClient](NodeClient.md), a do ponto de distribuição em [Distribuição](Distribuicao.md), e os algoritmos em [Algoritmos](Algoritmos.md). A verificação dos dados está em [Verificação e Incentivos](Verificacao%20e%20Incentivos.md); a governança, a auditabilidade e a privacidade em [Governança e Privacidade](Governanca%20e%20Privacidade.md); o cálculo do plano em [Planos de Produção](Planos%20de%20Producao.md).

## 1. Princípio operacional: plano vinculante, decisão local, consentimento sobre o trabalho

- **O SACCI não decide o plano; ele o despacha.** As ordens de suprimento (a nós a montante) e de produção (a cada nó) — quantidades, especificações, janelas de entrega, prioridades, substitutos técnicos autorizados, limites físicos — são a decomposição operacional do plano que a população votou.
- **Dentro das janelas e limites, a decisão é local.** O conselho de trabalhadores do nó define sequenciamento, turnos, mix e uso de substitutos.
- **Nenhuma alteração de intensidade ou de tempo de trabalho ocorre sem o consentimento do conselho do nó**, em votação, com prêmio tabelado. O aceite atualiza o plano central, que é recomputado e redistribuído. A recusa devolve a demanda ao SACCI para redistribuição (regras de prazo e de nós críticos em [Verificação e Incentivos](Verificacao%20e%20Incentivos.md)).

## 2. Arquitetura

### 2.1 Nós

Cada **nó produtivo** (unidade coletiva ou cooperativa) mantém um banco de dados local com: estoques de produtos e insumos; capacidade instalada e disponível; Lista de Materiais e coeficientes técnicos de cada item; tempos de processo; horas por trabalhador (contabilidade das pessoas, local) e horas totais (contabilidade do nó, pública); paradas e incidentes; consumo de energia, água e materiais; emissões e resíduos; ordens recebidas, início e fim de operações, entregas e recebimentos; conformidade de qualidade.

Cada **ponto de distribuição** registra a retirada de bens e serviços pela população (destruição de VT), agregada por bem, período e região antes de ser transmitida.

### 2.2 Coleta de dados

- Computadores de baixo custo em cada nó, integrados aos equipamentos onde houver automação (controladores, balanças, leitores, sensores de estoque, reconhecimento de imagem).
- Registro manual como último recurso, automatizado progressivamente.
- Todo evento de fluxo entre nós gera **dois lançamentos independentes** (saída no nó emissor, entrada no nó receptor); todo serviço gera um lançamento do prestador e uma **confirmação do destinatário**.
- Frequência: eventos são transmitidos ao ocorrer; agregados são consolidados por período de apuração (referência: diário para estoques e produção; mensal para horas e renda).

### 2.3 Comunicação

- Internet com protocolos padrão (HTTPS, mensageria assíncrona) ou rede dedicada; os protocolos de mensagem são públicos e versionados.
- Cada nó publica suas atualizações e assina os lançamentos com sua chave; cada nó consulta os dados dos nós de sua cadeia (a montante e a jusante).
- Os dados da contabilidade dos nós são **públicos**: qualquer cidadão, conselho ou corpo técnico pode consultá-los. Os dados individuais nunca saem do nó e do dispositivo do titular senão como totais com compromissos criptográficos ([Governança e Privacidade](Governanca%20e%20Privacidade.md)).

### 2.4 Computação

O sistema central mantém:

- A **matriz de coeficientes técnicos A** (insumo por unidade de produto), derivada das Listas de Materiais e atualizada pelos coeficientes observados.
- O vetor de **horas diretas** l por unidade de produto e as **horas embutidas** H = l(I − A)⁻¹.
- O **grafo produtivo** (nós e fluxos), com identificação de pontos de articulação (nós críticos sem redundância).
- O **plano vigente**: vetor de produção final y, produção bruta x, ordens, janelas, prioridades, limites, custos de plano e de equilíbrio.

Métodos:

- **Balanço material e horas embutidas**: sistemas lineares esparsos resolvidos por Gauss–Seidel ou Jacobi, com listas encadeadas; a esparsidade reduz a complexidade a algo próximo de O(n^1,4). Para dez milhões de produtos, o cálculo dos valores-trabalho é de minutos em hardware corrente e de frações de segundo em supercomputadores.
- **Otimização sob restrições**: programação linear (simplex esparso ou pontos interiores) para escolha entre técnicas alternativas, ativação de substitutos e realocação de carga, com a função objetivo votada e os limites físicos como restrições; os valores duais são publicados.
- **Projeção de demanda**: séries temporais do consumo agregado por bem e região, com sazonalidade; alimentam o custo de equilíbrio e o replanejamento.
- **Detecção de anomalias**: discrepâncias entre lançamentos pareados, desvios de coeficientes em relação a nós comparáveis, estoques fora do giro, capacidade declarada incoerente com produção histórica.

Há **implementações independentes** do núcleo de cálculo, mantidas por corpos técnicos distintos, executadas em paralelo a cada ciclo e comparadas; divergência é alerta público.

### 2.5 Distribuição da informação

O resultado da computação é devolvido à rede: cada nó recebe suas ordens e conhece a demanda prevista por seus produtos e a disponibilidade dos insumos de que depende; os corpos deliberativos recebem o plano recomputado, os valores duais e as anomalias; a população tem acesso aos dados dos nós, ao plano, aos custos e às simulações. Toda ordem é **explicável**: o nó pode consultar a cadeia de restrições, prioridades e demandas que a produziu.

## 3. Coleta do consumo final

O consumo final é registrado no ato, pela destruição de VT nos pontos de distribuição, e não estimado. Ele entra no SACCI agregado por bem, período e região. É a fonte da regra de custo de equilíbrio e da projeção de demanda. Os dados individuais de consumo permanecem no dispositivo do titular.

## 4. Cooperativas e unidades coletivas

Cooperativas ([Cooperativas](Cooperativas.md)) e unidades coletivas são nós iguais perante o SACCI: mesmas entradas obrigatórias, mesmos sinais vinculantes, mesma decisão local. Diferem no mandato (capilaridade e cadeias curtas versus infraestrutura e insumos críticos) e no regime do direito de uso.

## 5. Resposta a choques

Alterações drásticas de demanda ou de oferta — situações emergenciais, epidemias, falha de um nó, "cisnes negros" — são absorvidas por recomputação em poucos instantes:

1. **Detecção**: o evento é registrado pelos dados do próprio nó (parada, estoque, produção) ou pela anomalia nos fluxos.
2. **Cálculo de impacto**: o SACCI recalcula o balanço material e projeta o efeito em cascata (déficits por nó e por data) e a capacidade ociosa dos nós substitutos.
3. **Ação automática dentro do plano**: ativa substitutos técnicos previamente autorizados; redespacha insumos e capacidade onde isso está dentro das janelas e limites vigentes; aciona o fundo de reserva material.
4. **Proposta aos conselhos**: envia aos nós capazes de cobrir o déficit uma proposta de realocação de carga, com o prêmio tabelado; os conselhos aceitam ou recusam em prazo fixo.
5. **Atualização**: aceites atualizam o plano central; recusas devolvem a demanda para redistribuição; nós críticos seguem o regime especial.
6. **Publicação**: causa, impacto, ações e decisões são publicados.

Exemplo: incêndio em uma fábrica de borracha. O SACCI projeta que as fábricas de pneus terão déficit em X dias e as montadoras em Y; ativa borracha sintética onde autorizada; informa as demais fábricas de borracha da demanda não atendida; seus conselhos votam a intensificação com o prêmio tabelado; as fábricas de pneus ajustam seus planos dentro das janelas. A correção flui de baixo para cima com informação precisa, em vez de por comandos ou pela reação ambígua de um preço.

## 6. Requisitos não funcionais

- Disponibilidade: redundância geográfica do sistema central; operação degradada dos nós em caso de partição (fila local de lançamentos, sincronização posterior).
- Integridade: lançamentos assinados, imutáveis, encadeados por nó; contagens e cálculos reproduzíveis a partir dos dados públicos.
- Transparência: código público; dados dos nós públicos; modelos e parâmetros publicados com cada plano.
- Privacidade: dados individuais nunca centralizados em claro; chaves de trabalho, consumo e voto separadas e não vinculáveis.
- Auditabilidade: qualquer implementação independente deve reproduzir os resultados a partir dos mesmos dados públicos.
