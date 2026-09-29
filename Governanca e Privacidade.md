# Governança, Auditabilidade e Privacidade do SACCI

Se o SACCI é o sistema nervoso da economia, quem escreve o otimizador, escolhe os pesos da função objetivo, define os substitutos autorizados e mantém o código detém um poder que nenhuma votação alcança diretamente. "Dados públicos" não equivale a "modelo compreensível". A mesma lição do OGAS soviético — quem controla a informação controla o plano — vale para quem controla a infraestrutura. Este documento fixa como esse poder é limitado e como a transparência da produção convive com a privacidade da pessoa.

## 1. Governança do sistema

### 1.1 Código e modelos públicos
- Todo o software do SACCI — coleta, comunicação, cálculo, distribuição, votação — é público, versionado e auditável.
- A **função objetivo** de cada plano, os **pesos**, os **substitutos autorizados**, as **regras de custo de equilíbrio** (ganho, amortecimento, limites) e todos os parâmetros são **parte do plano votado** e publicados com ele. Não são detalhes de implementação.
- Toda mudança de código que afete o cálculo do plano é publicada com antecedência, com sua simulação de impacto, e entra em vigor por decisão por exceção na esfera nacional ([Democracia Direta Digital](Democracia%20Direta%20Digital.md), 7.2).

### 1.2 Implementações independentes
- Há pelo menos k implementações independentes do núcleo de cálculo (referência: k = 3), mantidas por corpos técnicos distintos, escolhidos por esferas distintas.
- A cada ciclo, todas executam o mesmo cálculo sobre os mesmos dados públicos; os resultados são comparados; divergência acima de tolerância é alerta público e suspende a decomposição em ordens até resolução.
- As simulações de opções de voto e a contagem de votos são executadas por todas as implementações.

### 1.3 Explicabilidade e contestação de ordens
- Toda ordem emitida a um nó é **explicável**: o nó tem direito à cadeia de demandas, restrições, prioridades e valores duais que a produziu.
- Um nó pode **contestar** uma ordem perante a esfera competente, em prazo fixo; a ordem contestada permanece vigente até decisão, salvo se violar limite físico ou de trabalho.
- As contestações e suas decisões são públicas e alimentam a revisão das regras.

### 1.4 Corpo técnico do SACCI
- Escolhido como os demais corpos técnicos (eleição pelos conselhos profissionais ou sorteio entre habilitados), com **mandato curto** (referência: 2 anos), **rotação obrigatória** e **recall**.
- Não tem poder sobre parâmetros nem sobre a função objetivo; apenas implementa o que foi votado e mantém a infraestrutura.
- Sua atuação é registrada (mudanças de código, intervenções operacionais) e auditável.

### 1.5 Centralização do cálculo, descentralização do controle
A infraestrutura é centralizada no sentido de haver um cálculo consistente para toda a economia. Seu controle não é: implementações plurais, parâmetros votados, ordens contestáveis e corpo técnico revogável.

## 2. Privacidade: duas contabilidades

Um sistema que registrasse cada hora e cada ato de consumo de cada pessoa em um banco central seria o maior aparato de vigilância já construído. O modelo separa, por arquitetura, o que o plano precisa saber do que ninguém precisa saber.

### 2.1 Contabilidade dos nós (pública)
Produção, estoques, capacidade, fluxos entre nós, horas **totais** por nó, coeficientes técnicos, consumo de energia e materiais, emissões, ordens, entregas, conformidade. É sobre ela que operam a partida dobrada, a comparação de coeficientes, a auditoria pelos nós a jusante e o cálculo do plano.

### 2.2 Contabilidade das pessoas (privada)
Quem trabalhou quantas horas, quem consumiu o quê, quem poupou, quem comprometeu vales em que proposta, quem votou em quê. Regras:

- Os registros individuais ficam **no nó** (horas) e **no dispositivo do titular** (consumo, poupança, compromissos), sob a chave do titular.
- O sistema central recebe apenas **totais por nó e por ponto de distribuição**, acompanhados de **compromissos criptográficos**: provas de que as parcelas individuais somam o total declarado sem que as parcelas sejam reveladas (compromissos homomórficos e provas de conhecimento nulo, técnica corrente).
- O consumo chega ao SACCI **agregado por bem, período e região**; a granularidade mínima de agregação é parâmetro (referência: nenhuma célula com menos de 100 pessoas é reportada isoladamente).
- O pré-compromisso em vales ([Inovação e Entrada](Inovacao%20e%20Entrada.md)) é **pseudônimo** para o SACCI, que só precisa do total reservado por proposta.
- A auditoria física de um nó examina estoques, máquinas e capacidade, **nunca** registros individuais.
- A dissidência interna não usa dados do sistema: quem denuncia já sabe.

### 2.3 Três chaves não vinculáveis
Cada cidadão tem três identidades criptográficas distintas, emitidas pela célula e **não vinculáveis** entre si nem pelo sistema central:

| Chave | Uso | Quem vê |
|---|---|---|
| **Trabalho** | Registro de horas no nó; recebimento de renda | O nó (horas) e o titular |
| **Consumo** | Destruição de VT; poupança; pré-compromisso | O titular; o ponto de distribuição vê apenas a validade do vale |
| **Voto** | Voto, delegação, contestação, assinatura de iniciativas | Contagem verificável sem recibo; delegações recebidas são públicas |

O recall e as iniciativas exigem identidade de voto; a renda exige identidade de trabalho; nenhuma delas dá acesso à outra.

### 2.4 Acesso por exceção
Nenhum corpo técnico ou executivo tem acesso a dados individuais por padrão. Um acesso individual só é possível por decisão da esfera do titular, em caso previsto em lei votada (por exemplo, fraude comprovada por auditoria física), com registro público do acesso e notificação ao titular.

## 3. Verificação e privacidade não competem

Quanto mais verificação, mais dados o sistema parece precisar. A tensão se desfaz porque a verificação opera sobre a **contabilidade dos nós**, que é pública, e a privacidade protege a **contabilidade das pessoas**, que não é necessária ao plano. Onde as duas se tocam — as horas por trabalhador que determinam a renda —, o registro fica no nó e com o titular, e o centro recebe o total com compromisso. Verificação e privacidade só competem se o desenho as confundir.

## 4. Segurança

- Lançamentos assinados, imutáveis e encadeados por nó; reprodutibilidade dos cálculos a partir dos dados públicos.
- Redundância geográfica; operação degradada dos nós em partição.
- Votação com verificabilidade de ponta a ponta, sem recibo, com revoto e alternativa em papel ([Democracia Direta Digital](Democracia%20Direta%20Digital.md), seção 8; [VoteSystem](VoteSystem.md)).
- Auditorias de segurança públicas e periódicas por equipes independentes; recompensas tabeladas por vulnerabilidades reportadas.
