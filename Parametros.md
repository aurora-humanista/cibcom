# Parâmetros

Todos os parâmetros quantitativos do modelo são votados de uma vez, na **lei de parâmetros**, em pacote único com o plano de curto prazo, com valores padrão herdados do período anterior ([Democracia Direta Digital](Democracia%20Direta%20Digital.md), 7.1). Esta tabela é a lista canônica: símbolo, significado, quem o fixa, valor de referência para simulação e documento em que é usado. Os valores de referência **não são recomendações**: são pontos de partida para o [Programa de Verificação](Programa%20de%20Verificacao.md).

## Distribuição

| Símbolo | Significado | Fixado por | Referência | Documento |
|---|---|---|---|---|
| d | Taxa de dedução social | Calculado pelo SACCI dados m, B, Q — ou votado, com Q/B ajustados | 0,35–0,50 (a medir com dados reais) | Planos de Produção §6 |
| B (= b · renda mediana) | Renda social básica por cidadão | Votado (b) | b = 0,3–0,5 | Tokens de Valor §3 |
| Suplementos de B | Para idade e deficiência, por avaliação | Votado (tabela) | — | Tokens de Valor §3 |
| mᵢ | Multiplicador de renda | 1 por padrão; prêmios tabelados | penosidade 1,1–1,5; escassez 1,1–1,3; criticidade 1,2 | Tokens de Valor §2 |
| Prêmio por alteração | Por tipo (hora extra, turno, antecipação, intensificação) | Votado (tabela) | 1,25–1,5 sobre as horas afetadas | Verificação e Incentivos §3 |
| s_max | Teto de poupança finalista (× renda anual) | Votado | 0,5 | Tokens de Valor §4 |
| T_s | Prazo máximo de cada parcela poupada | Votado | 24 meses | Tokens de Valor §4 |
| Cuidado por titularidade | Horas fixas por dependente, por faixa/avaliação | Votado (tabela) | — | Tokens de Valor §6 |

## Custos sociais

| Símbolo | Significado | Fixado por | Referência | Documento |
|---|---|---|---|---|
| FAAⱼ | Fator de Ajuste Ambiental por classe de impacto, normalizado | Votado (classes) | classes de 0,8 a 1,5 antes da normalização | Planos de Produção §5.2 |
| Ganho do custo de equilíbrio | Sensibilidade do ajuste ao desvio de giro | Votado | 0,1–0,3 por período | Planos de Produção §5.3 |
| Limite de variação (Δ_max) | Variação máxima de Pⱼ por período | Votado | ±10% | Planos de Produção §5.3 |
| η | Elasticidade da quantidade planejada à razão P/P* | Votado | 0,5 | Algoritmos §4 |
| Limiar de replanejamento setorial | Variação de proporções setoriais que exige decisão por exceção | Votado | 5% | Planos de Produção §7 |

## Plano e restrições

| Símbolo | Significado | Fixado por | Referência | Documento |
|---|---|---|---|---|
| Função objetivo | Critério do otimizador e pesos | Votado com o plano | minimizar horas ponderadas por penosidade s.a. mínimos e limites | Planos de Produção §3 |
| Limites físicos | Tetos de emissões, água, materiais, solo, por esfera | Votado | por esfera | Planos de Produção §2 |
| Mínimos | Níveis de atendimento por domínio | Votado, com base nos padrões dos Mínimos Cibercomunistas | por domínio | Mínimos Cibercomunistas |
| r | Redundância mínima de fornecedores de insumos críticos | Votado | 2 | Verificação e Incentivos §3 |
| Fundo de reserva | Fração das horas para reserva material e absorção de erro | Votado | 3–5% | Planos de Produção §6 |

## Democracia

| Símbolo | Significado | Fixado por | Referência | Documento |
|---|---|---|---|---|
| q | Fração mínima de participação para quórum | Votado | 0,5 | Quórum Mínimo §1 |
| a, a' | Adesão mínima (1ª preferência; duas primeiras) | Votado | 0,3; 0,5 | Quórum Mínimo §1 |
| R | Rodadas sem decisão até dissolução do CDT | Votado | 3 | Quórum Mínimo §2 |
| T_r | Duração de uma rodada | Votado | 5–10 dias | Quórum Mínimo §4 |
| Assinaturas para opção popular / iniciativa / recall | Fração do eleitorado da esfera | Votado | 1–2% | Democracia Direta Digital §6, §4 |
| T_c, f_c | Prazo e fração para contestar decisão por exceção | Votado | 15 dias; 2% | Democracia Direta Digital §7.2 |
| D_max | Teto de delegações por pessoa | Votado | 50 (célula) / 500 (município) | Democracia Direta Digital §7.3 |
| n_p | Tamanho do minipúblico | Votado | 50–200 por esfera | Democracia Direta Digital §7.4 |
| V_max | Teto de votações por cidadão por mês | Votado | 4 | Democracia Direta Digital §7.5 |
| ε | Limiar de variação de fluxo para atribuição de esfera | Votado | 1% | Democracia Direta Digital §3 |
| Mandato dos corpos técnicos | Duração e rotação | Votado | 2 anos, rotação de 1/2 por ano | Democracia Direta Digital §4 |

## SACCI e verificação

| Símbolo | Significado | Fixado por | Referência | Documento |
|---|---|---|---|---|
| k | Implementações independentes do núcleo | Votado | 3 | Governança e Privacidade §1.2 |
| Tolerância de divergência | Entre implementações | Votado | 0,1% | Governança e Privacidade §1.2 |
| Granularidade mínima de agregação | Menor população reportada isoladamente | Votado | 100 pessoas | Governança e Privacidade §2.2 |
| T_a | Prazo de resposta do conselho a alteração | Votado | 48 h (choque) / 10 dias (ordinário) | Verificação e Incentivos §3 |
| Períodos de apuração | Estoques/produção; horas/renda | Votado | diário; mensal | SACCI §2.2 |
| k_c | Limiar (em MAD) para anomalia de coeficiente | Votado | 3 | Algoritmos §9 |
| k_s | Períodos de consumo em estoque que caracterizam superprodução | Votado | 3 | Algoritmos §9 |
| ε_x, ε_H | Tolerâncias de convergência dos métodos iterativos | Técnico, publicado | 10⁻⁶ | Algoritmos §1–§2 |
| w_min | Peso mínimo no sorteio ponderado da fila de experimentação | Votado | equivalente a 5% do maior compromisso | Algoritmos §7 |

## Inovação e entrada

| Símbolo | Significado | Fixado por | Referência | Documento |
|---|---|---|---|---|
| E | Orçamento de experimentação (fração das horas e insumos por setor) | Votado (é uma das opções da cédula) | 3–10% | Inovação e Entrada §1 |
| n, m | Períodos de P/P* > 1 e de estagnação de coeficientes para elevar E no setor | Votado | 3; 6 | Inovação e Entrada §1 |
| T_v | Prazo de veto | Votado | 15 dias | Inovação e Entrada §2 |
| Prazo do pré-compromisso | | Votado | 60 dias | Inovação e Entrada §3 |
| T_p, n_e | Prazo do piloto; períodos de demanda sustentada | Votado | 12 meses; 3 | Inovação e Entrada §4 |
| Tempo protegido | Fração das horas de cada conselho para experimentação interna | Votado | 5% | Inovação e Entrada §5 |
| Prêmio ao inovador | Fração das horas economizadas; teto | Votado | 10%; teto = 2 × renda anual mediana | Inovação e Entrada §8 |

## Comércio exterior

| Símbolo | Significado | Fixado por | Referência | Documento |
|---|---|---|---|---|
| Balanço externo | Exportações, importações, saldo-alvo, reservas | Votado com o plano | — | Comércio Exterior §1 |
| e | Moeda estrangeira por hora exportada | Observado, publicado | — | Comércio Exterior §2 |
| Teto de conversão | Saldos monetários convertidos em VT na transição | Votado | — | Comércio Exterior §3 |
