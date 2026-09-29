# Algoritmos

Definições operacionais dos algoritmos referidos na documentação. Notação em [README](README.md). Toda implementação do núcleo ([SACCI-Core](SACCI-Core.md)) deve reproduzir estes resultados a partir dos mesmos dados públicos; as implementações independentes são comparadas sobre eles.

## 1. Balanço material (produção bruta)

Dado o vetor de produção final y (n bens) e a matriz de coeficientes técnicos A (a_ij = quantidade do bem i por unidade do bem j), a produção bruta x satisfaz x = Ax + y, isto é, x = (I − A)⁻¹y.

**Condição de existência.** A economia é produtiva (Hawkins–Simon): o raio espectral ρ(A) < 1. O SACCI verifica ρ(A) a cada atualização de A; ρ(A) ≥ 1 significa que algum ciclo produtivo consome mais do que produz e é publicado como anomalia estrutural.

**Gauss–Seidel esparso.**
```
x ← y                                  # chute inicial
repita
    para i = 1..n:
        s ← y_i
        para cada (j, a_ij) na linha i de A (apenas não nulos):
            s ← s + a_ij · x_j         # usa x_j já atualizado nesta iteração se j < i
        x_i ← s
até max_i |x_i − x_i^anterior| / max(1, x_i) < ε_x
```
Custo por iteração: O(nnz(A)). Convergência garantida para A ≥ 0 com ρ(A) < 1. Referência: ε_x = 10⁻⁶; iterações típicas < 50.

## 2. Horas embutidas (valores-trabalho)

Com l o vetor de horas diretas por unidade, as horas embutidas H satisfazem H = l + HA (H é vetor-linha), isto é, H = l(I − A)⁻¹.

**Iteração de Jacobi (Cockshott–Cottrell).**
```
H ← l
repita
    para j = 1..n:
        H_j^novo ← l_j + Σ_i H_i · a_ij     # soma sobre a coluna j de A (não nulos)
    H ← H^novo
até max_j |H_j^novo − H_j| / max(ε, H_j) < ε_H
```
A reposição dos meios de produção desgastados entra em A como coeficiente de depreciação física (unidades de máquina consumidas por unidade de produto), de modo que H já inclui a reposição. Horas de formação entram como l de um "bem" formação consumido pelo trabalho qualificado, quando a LM do item o declara.

## 3. Otimização sob restrições (programação linear)

Quando há técnicas alternativas para um mesmo bem, substitutos ou restrições físicas ativas, o plano é o problema:

```
minimizar   c · x
sujeito a   (I − A) x ≥ y            # atende a produção final
            R x ≤ r                  # limites físicos por esfera (emissões, água, materiais, solo)
            L x ≤ h                  # horas disponíveis por qualificação e região
            M x ≥ m                  # níveis mínimos (Mínimos Cibercomunistas)
            x ≥ 0
```
- Cada técnica alternativa é uma coluna própria (o mesmo bem pode ter várias colunas; y é atendido pela soma).
- c é a função objetivo votada: horas totais (c = l), horas ponderadas por penosidade (c = l ∘ w), emissões, ou combinação com pesos publicados.
- Resolução por simplex esparso ou pontos interiores. Os **valores duais** das restrições ativas (custo de oportunidade, em unidades de c, de relaxar cada limite em uma unidade) são publicados com o plano.
- Infactibilidade é resultado válido e público: indica que os mínimos e limites votados não cabem na capacidade; o CDT deve submeter opções que os relaxem ou que ampliem capacidade.

## 4. Custos sociais

**Custo de plano.** P*_j = FAA_j · H_j.

**Normalização do FAA.** Dados os fatores brutos f_j votados por classe de impacto (f_j = 1 para a classe neutra):
```
λ ← (Σ_j H_j Q_j) / (Σ_j f_j H_j Q_j)
FAA_j ← λ · f_j
```
Recomputado a cada replanejamento (Q muda). Propriedade: Σ_j FAA_j H_j Q_j = Σ_j H_j Q_j.

**Custo de equilíbrio.** Por bem de consumo j, por período t, com demanda observada D_j(t) (VT destruídos / P_j) e oferta planejada O_j(t):
```
δ ← (D_j(t) − O_j(t)) / O_j(t)                       # desvio de giro
Δ ← clamp(g · δ, −Δ_max, +Δ_max)                     # ganho g e limite por período
P_j(t+1) ← P_j(t) · (1 + Δ)
P_j(0) ← P*_j
```
Sinal para o plano seguinte: ρ_j = P_j / P*_j. Regra de replanejamento: Q_j(plano+1) = Q_j(plano) · ρ_j^η, com η (elasticidade de resposta) parâmetro, sujeito às restrições de §3. O amortecimento (g < 1, Δ_max) evita oscilação; o [Programa de Verificação](Programa%20de%20Verificacao.md) mede a convergência.

## 5. Fechamento e taxa de dedução

Dados m, h, B, N, ΔS, FAA, H, Q:
```
consumo_individual ← Σ_j FAA_j H_j Q_j
renda_bruta        ← Σ_i m_i h_i
d ← 1 − (consumo_individual + ΔS − B·N) / renda_bruta
```
Validações publicadas: 0 ≤ d < 1; d · renda_bruta ≥ horas dos fundos comuns (investimento líquido, reserva, administração, Mínimos, B·N, cuidado). Se a segunda falha, o plano é infactível no fechamento e volta ao CDT. Se a lei de parâmetros fixa d, resolve-se para Q (escalar o vetor de consumo individual) ou para B, conforme a variável declarada livre.

**Projeção de ΔS.** ΔS = S_nova − S_vencida − S_usada, onde S_vencida é conhecida (calendário de prazos) e S_nova e S_usada são projetadas por série temporal das contas; o fundo de reserva cobre o erro.

## 6. Apuração de votações

**Entrada.** Opções O = {o_1..o_k} mais o_0 ("nenhuma"); votos com preferência ordenada (ordenação parcial permitida: opções não listadas são empatadas abaixo das listadas); eleitorado E; participantes V.

```
para cada par (a, b): pref[a][b] ← nº de votos que colocam a acima de b
condorcet(o) ← ∀ b ≠ o: pref[o][b] > pref[b][o]
quorum ← |V| ≥ q · |E|
adesao(o) ← primeiras(o)/|V| ≥ a  ou  duas_primeiras(o)/|V| ≥ a'
validada(o) ← condorcet(o) ∧ quorum ∧ adesao(o)
```
Há no máximo uma opção validada. Se nenhuma, não há decisão. **Mais próxima do consenso** (usada após 2R rodadas): copeland(o) = Σ_{b≠o} (pref[o][b] − pref[b][o]); vence o maior; empate resolvido por primeiras(o).

**Delegação.** Antes da contagem, cada voto delegado no tema da cédula é substituído pelo voto do delegado. A delegação **não é transitiva**: um delegado vota com os votos que recebeu diretamente e não pode redelegá-los. Votos delegados contam em |V|. Planos, lei de parâmetros, recall e dissolução não admitem delegação.

## 7. Sorteios auditáveis

Todo sorteio (minipúblicos, fila de experimentação, corpos técnicos por sorteio) usa uma semente pública verificável: hash da concatenação de contribuições publicadas com antecedência por várias partes (esferas, implementações do SACCI), revelada após o fechamento das inscrições. O sorteio é reproduzível por qualquer pessoa.

**Sorteio ponderado sem reposição** (fila de experimentação): pesos w_i = w_min + VT comprometidos_i; sorteia-se sequencialmente com probabilidade w_i / Σ w, removendo o sorteado.

## 8. Grafo produtivo e nós críticos

Grafo G com nós produtivos e arestas de fluxo (i → j se i fornece a j). Nó crítico: **ponto de articulação** do grafo não dirigido subjacente (algoritmo de Tarjan, O(V + E)) **e** sem substituto autorizado nem fornecedor alternativo para o insumo que provê. A lista é recomputada a cada atualização do grafo e publicada; entrada e saída do regime especial seguem [Verificação e Incentivos](Verificacao%20e%20Incentivos.md) §4.

## 9. Detecção de anomalias

- **Partida dobrada.** Para cada `Transfer` de saída sem entrada correspondente (ou com quantidade divergente acima de tolerância) após o prazo de confirmação: anomalia, atribuída a ambos os nós até auditoria.
- **Coeficientes.** Para cada bem produzido por ≥ 3 nós e cada insumo: mediana e desvio absoluto mediano (MAD) dos coeficientes observados; nó com |coef − mediana| > k_c · MAD é sinalizado (k_c parâmetro; referência 3). Coeficientes observados = insumos consumidos / produto entregue no período.
- **Capacidade.** Capacidade declarada < máximo observado nos últimos p períodos → anomalia; capacidade declarada > 1,5 × máximo observado sem alteração de LM ou de equipamento registrada → sinalização para auditoria.
- **Giro.** Estoque acima de k_s períodos de consumo planejado → superprodução; custo de equilíbrio no limite superior com estoque zero por ≥ 2 períodos → subplanejamento (diagnóstico de mercado informal).

## 10. Atribuição de esferas por fluxos

Para uma proposta que altera o vetor y ou A, o SACCI calcula Δx (diferença de produção bruta com e sem a proposta) e marca as células cujos nós têm |Δx_j| / x_j > ε; junta as células do critério territorial; a esfera é a menor que contém todas ([Democracia Direta Digital](Democracia%20Direta%20Digital.md) §3).

## 11. Teto de atenção

Votações esperadas por cidadão por mês na esfera = (votações de plano e lei de parâmetros + contestações esperadas + recalls e iniciativas esperadas) / meses, com as contestações estimadas pela taxa histórica de contestação das decisões por exceção. Publicado com o plano; comparado a V_max.
