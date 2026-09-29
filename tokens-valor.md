# Tokens de Valor (vales-trabalho)

> Na produção socializada o capital monetário deixa de existir. A sociedade distribui a força de trabalho e os meios de produção entre os diversos ramos de negócios. Os produtores podem, por exemplo, receber vales de papel, com os quais podem retirar dos estoques sociais de consumo a quantidade de produtos correspondente ao seu tempo de trabalho. Esses vales não são dinheiro. Não circulam.
> — Karl Marx, *O Capital*, Livro II, cap. 18

> O produtor individual recebe de volta da sociedade — após as deduções — exatamente o que lhe deu. [...] Ele recebe da sociedade um certificado de que forneceu tal quantidade de trabalho e, com esse certificado, retira dos estoques sociais de meios de consumo o equivalente.
> — Karl Marx, *Crítica do Programa de Gotha*

## 1. Definição

O **vale-trabalho pessoal (VT)** é um direito individual de consumo, emitido pelo sistema central a cada cidadão, com quatro propriedades:

| Propriedade | Regra | Consequência |
|---|---|---|
| **Base no trabalho** | 1 VT por hora trabalhada, igual para todos (mᵢ = 1), com prêmios tabelados por penosidade, escassez temporária e criticidade | Não há prêmio por qualificação: a formação é custeada pela sociedade, com estudo remunerado, logo não há custo privado de educação a compensar |
| **Intransferível** | Só o titular pode usá-lo; não há transferência entre pessoas | Não é meio de circulação; não pode virar capital |
| **Destruído no consumo** | Ao retirar um bem ou serviço, os VT correspondentes ao seu custo social são cancelados | A unidade produtiva não recebe VT; recebe ordens e insumos e registra o que entrega |
| **Não acumulável** | Não há entesouramento sem prazo; a poupança admitida é finalista, com teto e prazo (seção 4) | Não se formam fortunas em VT |

Os VT são emitidos como registro em uma conta pessoal, com chave própria, separada das chaves de voto e de trabalho ([Governança e Privacidade](Governanca%20e%20Privacidade.md)).

## 2. Renda do trabalho

- Renda bruta do trabalhador i no período: mᵢ · hᵢ.
- Renda líquida (VT emitidos): (1 − d) · mᵢ · hᵢ.
- A dedução social d é única, proporcional e visível no extrato de cada trabalhador ([Planos de Produção](Planos%20de%20Producao.md), seção 6).
- Multiplicadores mᵢ: 1 por padrão. Prêmios tabelados na lei de parâmetros: penosidade (por classe de tarefa), escassez temporária de uma função (ativado por regra quando a mão de obra requerida supera a disponível nos dados do SACCI, desativado quando a diferença some), criticidade (trabalhadores de nós em regime especial, ver [Verificação e Incentivos](Verificacao%20e%20Incentivos.md)). Prêmios por alteração de intensidade aceita pelo conselho são igualmente tabelados.
- Horas de formação (estudo, aprendizagem) são horas trabalhadas: remuneradas a mᵢ = 1.

## 3. Renda social básica

Todo cidadão recebe, trabalhe ou não, uma **renda social básica** B por período, em VT:

- B é fixado pela lei de parâmetros como fração b da renda mediana do trabalho (referência para simulação: b entre 0,3 e 0,5).
- Crianças recebem B por meio dos responsáveis; pessoas idosas e pessoas com deficiência recebem B mais um suplemento definido por avaliação de necessidade, tabelado.
- B é financiada pela dedução d e entra na equação de fechamento como B·N.
- B cobre o **consumo individual**; os bens e serviços dos [Mínimos Cibercomunistas](M%C3%ADnimos%20Cibercomunistas/introducao.md) (alimentação adequada, moradia, saúde, educação, acesso digital, cultura, mobilidade) são providos **gratuitamente, in natura**, como consumo coletivo do plano, e não consomem VT.

Isso dá conteúdo operacional à parte da *Crítica do Programa de Gotha* sobre os que não podem trabalhar: o piso de consumo de quem está fora da produção é votado, e não implícito.

## 4. Poupança finalista

A não acumulação distingue **acumulação** (entesouramento sem prazo, que forma fortunas) de **poupança finalista** (consumo diferido para um bem de custo alto):

- Cada conta pode manter um saldo S até um teto s_max · (renda anual do titular), com s_max parâmetro (referência: 0,5).
- Cada parcela poupada tem prazo máximo T_s (referência: 24 meses); ao vencer sem uso, expira.
- O SACCI conhece o calendário de vencimentos e projeta ΔS, a poupança líquida do período, que entra na equação de fechamento; o erro de projeção é absorvido pelo fundo de reserva.
- Não há crédito ao consumo: o consumo diferido é poupado, não antecipado.

## 5. Custo social dos bens de consumo

Cada bem ou serviço de consumo individual tem um **custo de plano** P*ⱼ = FAAⱼ · Hⱼ e um **custo de equilíbrio** Pⱼ, ajustado pelo SACCI para que o estoque gire no ritmo planejado. A razão Pⱼ/P*ⱼ é sinal de replanejamento, nunca de remuneração ([Planos de Produção](Planos%20de%20Producao.md), seção 5). Ao retirar o bem j, o titular tem Pⱼ VT destruídos.

## 6. Cuidado por titularidade

O trabalho de cuidado de dependentes é trabalho social, remunerado pelo fundo comum, mas não por horas autodeclaradas (as menos verificáveis de toda a economia). A regra é por **titularidade**:

- Um dependente registrado na célula — criança, por faixa etária; adulto, por avaliação de necessidade — gera um **direito fixo de horas de cuidado** por período, tabelado.
- As horas são repartidas entre os cuidadores declarados e pagas em VT a mᵢ = 1.
- O que se verifica é a existência e a condição do dependente, fatos públicos na célula; não o que ocorre dentro de casa.

## 7. Mercado informal de bens

Os VT são intransferíveis; os bens retirados não são. A troca informal de bens entre pessoas existirá e é **tolerada**, não criminalizada. O que o desenho garante é que ela não se acumula como capital: não há meio de circulação em que armazenar o ganho, não há crédito, e o custo de equilíbrio elimina a principal fonte de arbitragem (a diferença entre o custo social e o valor que as pessoas atribuem ao bem). Essa contenção é **condicional**: nas economias de escassez, o mercado informal cresceu onde o custo oficial errava. Por isso o SACCI monitora, por bem, a combinação de custo de equilíbrio no teto de variação e estoque zerado: é o diagnóstico de um bem mal planejado, e a resposta é o replanejamento.

## 8. Emissão e destruição — resumo do ciclo

1. O SACCI registra as horas por unidade (contabilidade dos nós) e cada nó registra as horas por trabalhador (contabilidade das pessoas).
2. Ao fim do período de apuração, a conta de cada cidadão recebe (1 − d)·mᵢ·hᵢ + B (+ suplementos, + cuidado por titularidade).
3. Ao retirar bens e serviços, Pⱼ VT são destruídos; o consumo agregado por bem, período e região alimenta o SACCI.
4. Parcelas poupadas dentro do teto ficam na conta com prazo; ao vencer, expiram.
5. A equação de fechamento garante que a soma emitida iguala o custo social dos bens de consumo individual do plano.
