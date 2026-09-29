# Quórum Mínimo

É necessário um quórum mínimo para que uma decisão seja validada: se não há consenso substancial, não há decisão. Este documento define formalmente a regra e suas consequências.

## 1. Definição

Seja uma votação com opções O = {o₁, …, oₖ}, votantes V (com preferência ordenada sobre O) e eleitorado E da esfera. Uma opção oᵢ é **validada** se e somente se:

1. **Condição de maioria (Condorcet).** Para toda outra opção oⱼ, o número de votantes que preferem oᵢ a oⱼ é maior que o número que prefere oⱼ a oᵢ.
2. **Condição de quórum.** |V| ≥ q · |E|, onde q é a fração mínima de participação (parâmetro da lei de parâmetros).
3. **Condição de adesão.** A opção oᵢ é a primeira preferência de pelo menos uma fração a (parâmetro) dos votantes, ou está entre as duas primeiras preferências de pelo menos uma fração a' (parâmetro).

Se nenhuma opção satisfaz as três condições, **não há decisão** nessa rodada. Votos delegados contam em |V| pelo delegado (a delegação não é transitiva e não vale para planos, lei de parâmetros, recall e dissolução). Abstenção e voto em branco contam para |V| apenas se o votante registrou participação; um voto em branco expressa preferência por "nenhuma das opções" e é tratado como opção implícita o₀ na condição 1.

A condição 3 impede que uma opção Condorcet-vencedora mas fracamente apoiada (todos a colocam em segundo) seja validada sem adesão real; a condição 1 impede que a fragmentação de opções manipule o resultado.

## 2. Rodadas e dissolução

- Se não há decisão, o Corpo Deliberativo Técnico (CDT) revisa as opções à luz dos resultados publicados (distribuição de preferências, recomendações dos minipúblicos, contestações) e convoca nova rodada.
- Após **R rodadas** sem decisão (parâmetro; valor de referência R = 3), o CDT responsável é **dissolvido automaticamente** e novas eleições ou sorteio são convocados na esfera.
- O novo CDT delibera e formula novas opções. Se, decorridas novamente R rodadas, ainda não houver decisão, a opção **mais próxima do consenso** é validada provisoriamente e novas eleições para o CDT são convocadas de qualquer modo.

"Mais próxima do consenso" é definida como a opção que maximiza a soma, sobre todas as demais opções, das margens de vitória em confronto par a par (critério de Copeland ponderado). A definição é pública e computada pelas implementações independentes do SACCI ([Algoritmos](Algoritmos.md) §6).

## 3. Validação provisória

Uma decisão validada provisoriamente vigora com as mesmas consequências de uma decisão válida, mas é obrigatoriamente resubmetida a voto na rodada seguinte de plano, com o novo CDT.

## 4. Prazos

O processo é digital: uma rodada dura T_r dias (parâmetro; referência 5 a 10 dias); um ciclo com dissolução e nova eleição dura semanas, não meses. Durante a ausência de decisão sobre um novo plano, o plano vigente continua a ser executado e replanejado pelo SACCI dentro dos seus próprios limites.

## 5. Efeito sobre a deliberação

A regra cria incentivo para que os ramos do CDT façam concessões entre si e incorporem a opinião popular já na elaboração: o custo de apresentar opções que a população rejeita é a perda do mandato. As recomendações dos minipúblicos e a publicação das distribuições de preferência a cada rodada fornecem ao CDT a informação para convergir.
