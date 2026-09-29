# Glossário

Termos do modelo, na ordem em que costumam aparecer. Notação matemática em [README](README.md).

**Irracionalidade produtiva** — desvio sistêmico entre o que a sociedade pode produzir e o que produz, decorrente da orientação da produção à valorização do capital e da correção *ex-post* pelo preço.

**Célula** — unidade basilar da democracia direta: grupo de cidadãos com entorno comum (bairro, unidade produtiva). Emite credenciais, opera o voto em papel, registra dependentes e direitos.

**Esfera de decisão** — conjunto de células afetadas por uma decisão; a menor esfera que contém todas as células identificadas pelos critérios de fluxos e de território. Município, região e país são esferas territoriais.

**Corpo Deliberativo Técnico (CDT)** — corpo de especialistas, escolhido por conselhos setoriais (eleição ou sorteio), que elabora as opções submetidas a voto. Não legisla; é revogável; é dissolvido automaticamente após R rodadas sem decisão.

**Corpo Executivo** — executa as decisões e planos aprovados, dentro dos limites votados; revogável.

**Corpo técnico do SACCI** — mantém a infraestrutura do SACCI; sem poder sobre parâmetros; mandato curto, rotação e recall.

**Recall** — destituição de um membro de corpo técnico ou executivo por voto da esfera, mediante petição.

**Quórum mínimo** — regra de validação: uma opção é validada se vence todas as outras par a par (Condorcet), a participação atinge q·|E| e a adesão atinge a (primeiras preferências) ou a' (duas primeiras).

**Lei de parâmetros** — pacote único, votado com o plano, com todos os parâmetros quantitativos do modelo ([Parâmetros](Parametros.md)).

**Decisão por exceção** — proposta que entra em vigor por padrão após prazo T_c, salvo pedido de votação por fração f_c da esfera.

**Delegação por tema** — cessão revogável do voto, em matérias definidas, a pessoa identificada, não transitiva, com teto D_max; não vale para plano, lei de parâmetros, recall e dissolução.

**Minipúblico** — painel sorteado de cidadãos da esfera, remunerado, que delibera e publica recomendação ao lado de cada opção de plano.

**Teto de atenção (V_max)** — número máximo de votações por cidadão por mês; um plano que o excede é inválido.

**Plano de produção** — vetor de produção final y, restrições (físicas, de trabalho, de mínimos, de atenção) e parâmetros distributivos, com horizonte definido; votado; decomposto em ordens pelo SACCI; replanejado continuamente.

**Mínimos Cibercomunistas** — piso material universal (alimentação, moradia, saúde, educação, acesso digital, cultura, mobilidade) provido *in natura*, gratuito, como consumo coletivo; restrição de validade do plano.

**Matriz de coeficientes técnicos (A)** — a_ij = quantidade do bem i por unidade do bem j; agregada das Listas de Materiais e dos coeficientes observados.

**Lista de Materiais (LM)** — insumos físicos, coeficientes, horas diretas e substitutos autorizados de um item, declarada pelo nó.

**Horas embutidas (H)** — horas diretas mais indiretas por unidade de bem: H = l(I − A)⁻¹.

**Custo social** — termo do modelo para o que no mercado seria "preço": **custo de plano** P* = FAA·H; **custo de equilíbrio** P, ajustado para que o estoque gire no ritmo planejado. A razão P/P* é sinal de replanejamento. Não há custo de troca entre unidades produtivas.

**FAA (Fator de Ajuste Ambiental)** — multiplicador do custo de plano por classe de impacto, votado e normalizado (média ponderada 1).

**Vale-trabalho pessoal (VT)** — direito individual de consumo: 1 por hora, intransferível, destruído no consumo, não acumulável (poupança finalista com teto e prazo).

**Renda** — VT recebidos: renda do trabalho (1 − d)·mᵢ·hᵢ, renda social básica B, suplementos, cuidado por titularidade.

**Multiplicador de renda (mᵢ)** — 1 por padrão; prêmios tabelados por penosidade, escassez temporária, criticidade e alteração de intensidade.

**Taxa de dedução social (d)** — fração das horas retida para os fundos comuns (investimento, reserva, administração, Mínimos, renda básica, cuidado); calculada pelo SACCI ou votada.

**Renda social básica (B)** — dotação em VT a todo cidadão, fração b da renda mediana, financiada por d.

**Poupança finalista (S)** — saldo de VT com teto s_max e prazo T_s; ΔS é sua variação líquida no período.

**Equação de fechamento** — (1 − d)·Σ mᵢhᵢ + B·N − ΔS = Σ FAAⱼHⱼQⱼ.

**Cuidado por titularidade** — remuneração do cuidado de dependentes por direito fixo de horas por dependente registrado, não por horas autodeclaradas.

**SACCI** — Sistema Automatizado de Coleta e Computação de Informação: coleta, matriz, planejamento, custos, verificação, distribuição de ordens e informação. **SACCI-Core** é sua implementação central; **NodeClient**, o software do nó; **ponto de distribuição**, o nó de consumo final.

**Nó produtivo** — unidade coletiva ou cooperativa conectada ao SACCI, com conselho de trabalhadores.

**Cooperativa** — nó com direito de uso condicionado sobre meios sociais, autogerido, com mandato de capilaridade e cadeias curtas.

**Ordem de produção / de suprimento** — decomposição operacional do plano votado: quantidade, especificação, janela, prioridade, substitutos, limites, explicação.

**Janela de entrega** — intervalo temporal com tolerâncias dentro do qual o nó decide sequenciamento e turnos.

**ChangeRequest (pedido de alteração)** — proposta do SACCI que exige mudança de intensidade ou tempo de trabalho; requer voto do conselho em prazo T_a, com prêmio tabelado; silêncio é recusa.

**Partida dobrada** — todo fluxo entre nós tem dois lançamentos independentes (saída e entrada); discrepância é anomalia.

**Contabilidade dos nós / das pessoas** — a primeira (produção, estoques, fluxos, horas totais, consumo agregado) é pública; a segunda (horas por pessoa, consumo individual, poupança, compromissos, votos) fica com o nó e o titular e chega ao centro só como totais com compromisso criptográfico.

**Compromisso criptográfico** — prova de que parcelas não reveladas somam um total declarado.

**Três chaves** — identidades criptográficas distintas e não vinculáveis: trabalho, consumo, voto.

**Nó crítico** — ponto de articulação do grafo produtivo sem substituto; sujeito ao regime especial (arbitragem vinculante, prêmio de criticidade, construção de redundância).

**Efeito catraca** — incentivo, nas economias de comando, a produzir abaixo da capacidade porque o cumprimento vira a meta seguinte; neutralizado por metas por comparação.

**Canal de contestação** — pedido de auditoria física de um fornecedor por nó a jusante, com custo imputado a quem estiver errado.

**Orçamento de experimentação (E)** — fração das horas e insumos de cada setor reservada a propostas fora do plano.

**Pré-compromisso em vales** — reserva pseudônima de VT em uma proposta do catálogo; destruídos só na entrega; demanda registrada quando cobre a primeira tiragem.

**Esteira de entrada** — portões protótipo → piloto → plano, com critérios objetivos, prazos e reversão automática.

**Tempo protegido** — fração das horas de cada conselho para experimentação interna sem autorização.

**Sorteio auditável** — sorteio com semente pública verificável; ponderado pelo pré-compromisso na fila de experimentação.

**Monopólio social do comércio exterior** — toda troca externa é operada pelo Executivo dentro de um balanço votado; moeda estrangeira não circula internamente; custo do importado em horas de exportação.

**Programa de verificação** — simulação com matrizes reais, simulação de agentes e experimentos, com critérios de refutação.
