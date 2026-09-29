# Planos de Produção

O plano de produção é o principal objeto de votação macroeconômica. Este documento especifica sua estrutura, seu cálculo, seus custos sociais, sua equação de fechamento e seu replanejamento.

## 1. Horizontes

| Horizonte | Conteúdo | Frequência de voto |
|---|---|---|
| Longo prazo (5–10 anos) | Direção estrutural: infraestrutura, energia, transição tecnológica, limites ecológicos de longo prazo, redundância de nós críticos | A cada ciclo, com revisão anual |
| Médio prazo (2–3 anos) | Investimento por setor, capacidade a instalar, formação de força de trabalho | Anual |
| Curto prazo (1 ano, referência) | Metas de produção por setor e produto, alocação de horas e insumos, renda básica, lei de parâmetros | Anual, com replanejamento contínuo pelo SACCI |

Os planos de horizonte menor são decompostos do de horizonte maior e devem ser consistentes com ele; uma inconsistência detectada pelo SACCI é publicada.

## 2. Estrutura do plano de curto prazo

O plano de curto prazo é um vetor de metas e um conjunto de restrições:

- **Vetor de produção final** y: quantidades de cada bem e serviço destinadas ao consumo individual (Qⱼ), ao consumo coletivo gratuito (saúde, educação, cultura), ao investimento (ampliação e reposição de meios de produção), à reserva e à exportação.
- **Restrições de mínimos**: níveis de atendimento mínimos por domínio dos [Mínimos Cibercomunistas](M%C3%ADnimos%20Cibercomunistas/introducao.md) (alimentação, moradia, saúde, educação, acesso digital, cultura, mobilidade), expressos em quantidades físicas e indicadores (por exemplo, kcal/pessoa/dia e prevalência de insegurança alimentar). Um plano que não os satisfaz é inválido; o CDT só pode submeter opções que os cumpram, e o SACCI verifica isso antes da publicação.
- **Restrições físicas**: tetos por período de emissões, uso de água, materiais críticos, uso do solo, por esfera.
- **Restrição de trabalho**: Σᵢ hᵢ ≤ horas socialmente disponíveis, com desagregação por qualificação e região.
- **Restrição de atenção**: número esperado de votações por cidadão por mês ≤ V_max.
- **Parâmetros distributivos**: d, B, FAA, multiplicadores, orçamento de experimentação (ver [Parâmetros](Parametros.md)).

## 3. Cálculo do plano

Dado o vetor de produção final y, o SACCI calcula ([Algoritmos](Algoritmos.md) §1–§3) a produção bruta necessária x pelo modelo de Leontief, x = (I − A)⁻¹ y, resolvido por métodos iterativos (Gauss–Seidel, Jacobi) que exploram a esparsidade de A, e as horas embutidas por unidade de cada bem, H = l (I − A)⁻¹, onde l é o vetor de horas diretas por unidade. Quando há escolha entre técnicas alternativas ou restrições físicas ativas, o problema é resolvido por programação linear: minimizar as horas totais (ou outra função objetivo votada) sujeito a (I − A)x ≥ y e às restrições físicas. Os multiplicadores das restrições ativas (valores duais) são publicados: eles medem o custo de oportunidade, em horas, de cada limite físico, e informam o CDT sobre onde a folga é mais valiosa.

A função objetivo é um **parâmetro votado**, publicado com o plano. Opções típicas: minimizar horas totais; minimizar horas ponderadas por penosidade; minimizar emissões sujeito a nível de atendimento; combinações com pesos.

## 4. Decomposição em ordens

O plano aprovado é decomposto pelo SACCI em **ordens de produção** para cada nó e **ordens de suprimento** para os nós a montante, com quantidade, especificação, janela de entrega, prioridade de atendimento e substitutos técnicos autorizados. As ordens são a operacionalização do plano votado; o SACCI não decide o plano, ele o despacha. A execução (sequenciamento, turnos, mix dentro das janelas) é do conselho da unidade ([SACCI](SACCI.md), seção 4).

## 5. Custos sociais

### 5.1 Custo de plano

Para cada bem de consumo j:

P*ⱼ = FAAⱼ · Hⱼ

onde Hⱼ são as horas embutidas (diretas + indiretas, via LM, incluindo a reposição dos meios de produção desgastados) e FAAⱼ é o Fator de Ajuste Ambiental.

### 5.2 Normalização do FAA

Os FAA são decididos democraticamente por classe de impacto e **normalizados** de modo que

Σⱼ FAAⱼ · Hⱼ · Qⱼ = Σⱼ Hⱼ · Qⱼ.

O FAA reordena custos relativos — bens de alto impacto ficam mais caros, bens de baixo impacto mais baratos — sem criar nem destruir poder de compra no agregado. A normalização é recomputada a cada replanejamento.

### 5.3 Custo de equilíbrio

O custo efetivo Pⱼ é ajustado pelo SACCI, por regra publicada, para que o estoque do bem j gire no ritmo planejado (oferta = demanda no período). Regra: ajuste proporcional ao desvio entre demanda observada e oferta planejada, com amortecimento (ganho g e limite de variação Δ_max por período são parâmetros) para evitar oscilação; a quantidade do plano seguinte responde à razão P/P* com elasticidade η ([Algoritmos](Algoritmos.md) §4).

A razão Pⱼ / P*ⱼ é o **sinal para o plano seguinte**:

- Pⱼ / P*ⱼ > 1: a população valoriza o bem acima do trabalho que ele custa → expandir a produção.
- Pⱼ / P*ⱼ < 1: → contrair.

O sinal é usado para **replanejar**, nunca para remunerar a unidade produtiva. Os custos de equilíbrio existem apenas para bens e serviços de consumo individual. **Não há custos de troca nem preços entre unidades produtivas**: meios de produção e insumos são alocados *in natura* pelo plano.

## 6. Equação de fechamento

Os vales disponíveis para consumo no período devem igualar o valor-trabalho dos bens de consumo individual do plano:

**(1 − d) · Σᵢ mᵢ·hᵢ + B·N − ΔS = Σⱼ FAAⱼ · Hⱼ · Qⱼ**

- (1 − d) · Σᵢ mᵢ·hᵢ: renda do trabalho líquida da dedução social.
- B·N: renda social básica (ver [Tokens de Valor](tokens-valor.md)).
- ΔS: variação líquida da poupança finalista, projetada pelo SACCI a partir dos saldos e vencimentos registrados.
- Lado direito: com FAA normalizado, é simplesmente Σⱼ Hⱼ · Qⱼ.

A dedução social financia: reposição não embutida e ampliação dos meios de produção, fundo de reserva, administração, os bens e serviços gratuitos dos Mínimos Cibercomunistas (consumo coletivo, sem custo em VT), a renda social básica e o cuidado por titularidade. Os Mínimos são providos *in natura* e não passam pela equação de fechamento; a renda básica B cobre o consumo individual além deles. Consistência: d · Σᵢ mᵢ·hᵢ = horas destinadas a esses fundos, inclusive B·N.

**Graus de liberdade.** Dados mᵢ, B e Qⱼ votados, o SACCI **calcula d** e o publica com o plano. Alternativamente, a população vota d (a "taxa de acumulação social") e o SACCI ajusta Qⱼ ou B. O plano submetido a voto exibe as leituras alternativas; a lei de parâmetros diz qual variável é livre.

**Curto prazo.** Se Σⱼ Pⱼ·Qⱼ (custos de equilíbrio) diferir de Σⱼ P*ⱼ·Qⱼ (custos de plano) dentro do período, a diferença é absorvida pelo **fundo de reserva** e corrigida no replanejamento; a convergência é garantida pela regra P/P* com amortecimento.

## 7. Replanejamento contínuo

O plano aprovado não é estático. O SACCI o recomputa continuamente com os dados de produção, estoque e consumo em tempo real, e a cada choque (ver [SACCI](SACCI.md), seção 5). Regras:

- Alterações **dentro** das janelas e limites do plano são despachadas automaticamente.
- Alterações que exigem de um nó **mudança de intensidade ou tempo de trabalho** exigem aprovação do conselho do nó, com prêmio tabelado ([Verificação e Incentivos](Verificacao%20e%20Incentivos.md)).
- Alterações que mudam as **proporções setoriais** além de um limiar (parâmetro) são submetidas à esfera por decisão por exceção ([Democracia Direta Digital](Democracia%20Direta%20Digital.md), 7.2).
- Todos os replanejamentos, com suas causas, são públicos.

## 8. Publicação

Cada plano é publicado com: o vetor y e as restrições; a matriz A e o vetor l usados; as horas embutidas H; os FAA e sua normalização; d, B e a equação de fechamento com seus termos; a função objetivo; os valores duais das restrições ativas; a carga de atenção prevista; as simulações de cada opção votada; e os resultados das implementações independentes.
