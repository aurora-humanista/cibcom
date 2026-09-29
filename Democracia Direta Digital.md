# Democracia Direta Digital

Este documento especifica a estrutura política do CiberCom: quem decide, sobre o quê, em que esfera, com que carga de atenção e com que garantias.

## 1. Princípios

1. **Soberania direta.** Todas as decisões legislativas e macroeconômicas são tomadas por voto direto da população. Não há representantes com poder legislativo.
2. **Deliberar não é decidir.** Corpos técnicos elaboram opções; corpos executivos implementam decisões; apenas a população decide. Ambos os corpos têm poder limitado, prestam contas e são revogáveis a qualquer momento.
3. **Correspondência entre impacto e eleitorado.** Uma decisão é tomada por todos os cidadãos da esfera que ela afeta, e apenas por eles.
4. **Atenção é um recurso escasso.** O número de decisões submetidas a cada cidadão é limitado por regra, e a maior parte das decisões de rotina vigora por padrão salvo contestação.
5. **Transparência da produção, privacidade da pessoa.** Os dados do plano e da produção são públicos; os dados da vida individual não são (ver [Governança e Privacidade](Governanca%20e%20Privacidade.md)).

## 2. Células e esferas de decisão

A unidade basilar é a **célula**: um grupo de cidadãos com entorno comum — um bairro, uma unidade produtiva — que delibera e vota sobre as questões desse entorno. Células compõem **esferas** maiores (município, região, país). Toda célula pertence a exatamente uma cadeia de esferas territoriais; um cidadão pertence a uma célula residencial e, se trabalha, ao conselho de sua unidade produtiva.

Exemplo: as células A, B, C, D e E (1.000 habitantes cada) formam a esfera do município. Uma decisão que afeta só A é votada pelos cidadãos de A. Uma decisão que afeta o município é votada por todos os cidadãos de A a E.

## 3. Atribuição de esferas

Definir a que esfera pertence uma decisão é, ela mesma, uma decisão política. Para que não seja prerrogativa de quem administra o sistema, a atribuição segue uma regra padrão contestável, com dois critérios:

- **Critério de fluxos.** O SACCI identifica, pela matriz insumo-produto e pelos registros de fluxo, as células cujas unidades têm entradas ou saídas materialmente afetadas pela proposta (variação prevista acima de um limiar ε, parâmetro).
- **Critério territorial.** O cadastro territorial identifica as células em cujo território a proposta se realiza fisicamente, incluindo as afetadas por uso do solo, corpos d'água, ruído, paisagem e patrimônio.

A esfera padrão é a **menor esfera que contém todas as células identificadas pelos dois critérios**. Qualquer célula que se considere afetada e não tenha sido incluída pode reivindicar a inclusão em prazo fixo; a reivindicação é decidida por voto da esfera imediatamente superior à disputada. A atribuição e as reivindicações são públicas.

## 4. Corpos representativos com poder limitado

### 4.1 Corpo Deliberativo Técnico (CDT)

- Composto por especialistas escolhidos por meio dos conselhos profissionais e setoriais (saúde, educação, engenharia, transporte, energia etc.), à maneira de um conjunto de ministérios **sem poder legislativo**.
- Função: elaborar as **opções** submetidas a voto — planos de produção, leis, parâmetros — com sua simulação pelo SACCI.
- Método de escolha: eleição pelos conselhos setoriais ou sorteio entre os habilitados; a esfera decide o método. Mandato curto, com rotação obrigatória.
- **Recall**: qualquer membro pode ser destituído a qualquer momento por voto da esfera que o escolheu, mediante iniciativa com número mínimo de assinaturas.
- **Dissolução automática**: ver [Quórum Mínimo](Quorum%20Minimo.md).

### 4.2 Corpo Executivo

- Executa as decisões e planos aprovados, dentro dos limites por eles fixados. Não decide metas nem parâmetros.
- Escolhido por eleição na esfera, com poder limitado, prestação de contas contínua (os dados de execução são os do SACCI) e recall nas mesmas condições do CDT.

### 4.3 Corpo técnico do SACCI

O corpo responsável pela manutenção do SACCI é um corpo técnico como os demais: eleito, revogável, com mandato curto e rotação. Suas atribuições e limites estão em [Governança e Privacidade](Governanca%20e%20Privacidade.md).

## 5. O que é votado

- **Planos de produção** de curto, médio e longo prazo ([Planos de Produção](Planos%20de%20Producao.md)).
- **Lei de parâmetros**: todos os parâmetros quantitativos do modelo, em pacote único com o plano ([Parâmetros](Parametros.md)).
- **Leis** em geral, na esfera a que pertencem.
- **Recalls** e **dissoluções**.
- **Contestações**: decisões que vigorariam por padrão e foram contestadas (seção 7).

Decisões microeconômicas (execução, sequenciamento, turnos, mix dentro das janelas do plano) são dos conselhos de trabalhadores das unidades e não vão a voto da esfera.

## 6. Enquadramento das opções

Quem escreve as opções influencia o resultado. Três regras limitam esse poder:

1. **Opção de origem popular.** Qualquer proposta que reúna, na esfera, um número mínimo de assinaturas (parâmetro, fração do eleitorado) entra na cédula ao lado das opções do CDT, com o mesmo direito à simulação pelo SACCI.
2. **Simulação obrigatória.** Cada opção é acompanhada de sua simulação: consequências projetadas em horas de trabalho, insumos, limites físicos, taxa de dedução d, renda básica B e nível de atendimento por setor. A simulação é executada por **todas as implementações independentes** do SACCI e publicada lado a lado; divergência entre implementações é alerta público. O grupo proponente de uma opção popular pode submeter a própria execução do modelo, com os dados públicos, que aparece na cédula ao lado das oficiais.
3. **Preferência ordenada.** O voto é por ordenação das opções, não por escolha única, o que impede que o quórum seja manipulado pela fragmentação das alternativas. A regra de decisão está em [Quórum Mínimo](Quorum%20Minimo.md).

## 7. Carga decisória

Um desenho que exija do cidadão a atenção de um parlamentar em tempo integral produz abstenção e captura por minorias mobilizadas. Quatro regras reduzem o número de decisões sem devolver o poder a representantes.

### 7.1 Lei de parâmetros

Todos os parâmetros quantitativos (d, B, FAA, multiplicadores, orçamento de experimentação, tetos e prazos de poupança, prazos de veto e contestação, limiares) são votados **de uma vez**, junto com o plano, em um pacote com valores padrão herdados do período anterior. Um parâmetro só volta a voto isolado por iniciativa com número mínimo de assinaturas.

### 7.2 Decisão por exceção

Dentro de cada esfera, uma proposta do CDT ou do Executivo que não seja plano, lei de parâmetros ou lei é **publicada com sua simulação e entra em vigor por padrão** após um prazo T_c (parâmetro), salvo se uma fração mínima f_c dos cidadãos da esfera (parâmetro) pedir votação dentro do prazo. O que ninguém contesta não é votado; o que alguém contesta é votado por todos. As propostas publicadas e as contestações são públicas.

### 7.3 Delegação revogável por tema

Um cidadão pode delegar seu voto, em **matérias definidas** (saúde, energia, transporte, educação etc.), a uma pessoa identificada. Regras:

- A delegação é revogável a qualquer momento, com efeito imediato.
- Os votos de quem recebe delegações são públicos.
- Há um teto D_max (parâmetro) para o número de delegações que uma pessoa pode acumular.
- A delegação não é transitiva: o delegado vota com os votos recebidos diretamente e não pode redelegá-los.
- Não há delegação para planos de produção, lei de parâmetros, recall e dissolução: esses votos são pessoais.

### 7.4 Minipúblicos

Para cada votação de plano e de lei de parâmetros, um **painel sorteado** de cidadãos da esfera (tamanho n_p, parâmetro), remunerado em vales pelas horas, delibera com acesso aos técnicos e às simulações e publica uma **recomendação fundamentada** ao lado de cada opção. O painel não decide; qualifica o voto de todos.

### 7.5 Teto de atenção

O SACCI calcula e publica, com cada plano, o **número esperado de votações por cidadão por mês** na esfera, somando votações diretas e contestações previstas. Um plano que exceda o teto V_max (parâmetro da lei de parâmetros) é inválido, como é inválido um plano que exceda o teto de emissões.

## 8. Segurança e sigilo do voto

O sistema de votação decide a alocação da economia e é o alvo mais valioso que se pode imaginar. Requisitos:

- **Verificabilidade de ponta a ponta.** Cada votante pode confirmar que seu voto foi registrado como emitido e incluído na contagem, **sem poder provar a terceiros como votou** (ausência de recibo), o que remove o incentivo à compra de votos e à coerção.
- **Código público e contagem redundante.** O software é público e auditável; a contagem é executada de forma independente pelas implementações paralelas do SACCI e os resultados comparados.
- **Revoto.** O votante pode votar quantas vezes quiser até o fechamento; vale o último voto. Isso esvazia a coerção presencial.
- **Alternativa presencial.** Para planos e leis de parâmetros, cada célula oferece voto presencial em papel, para quem preferir ou não tiver acesso; o resultado é consolidado com o eletrônico.
- **Separação de identidades.** A chave usada para votar é distinta e **não vinculável** às chaves usadas para trabalhar e consumir. O registro de votos nunca pode ser cruzado com o de consumo ou de trabalho.

## 9. Implementação

A especificação do software está em [VoteSystem](VoteSystem.md); a apuração e os sorteios em [Algoritmos](Algoritmos.md) §6–§7.

## 10. Processo digital

O processo é digital de ponta a ponta: a publicação de opções, simulações e recomendações, o voto, a contestação, a delegação e o recall são operações no mesmo sistema, com notificação ao cidadão. Uma rodada de votação de plano transcorre em dias; um ciclo completo com dissolução e nova eleição do CDT, em semanas.
