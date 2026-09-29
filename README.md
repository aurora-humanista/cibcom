# CiberCom

Modelo de planejamento econômico cibernético sob democracia direta digital, cálculo em unidades físicas e em tempo de trabalho, e distribuição por vales-trabalho.

## Proposta geral

O projeto responde a dois problemas:

- **A irracionalidade produtiva.** No capitalismo, a alocação da capacidade produtiva é corrigida *ex-post* pelo sinal de preço e orientada à valorização do capital, não à satisfação de necessidades. O CiberCom propõe que o planejamento da produção e da distribuição opere pela racionalidade humana — metas votadas, cálculo *in natura* e correção em tempo real — e não pela arbitrariedade de um mercado.
- **A democratização completa do poder de decisão.** A vontade da população é a única fonte de poder. Isso é realizado por uma democracia direta digital organizada em esferas de decisão, por corpos técnico e executivo com mandato limitado e revogável, e por conselhos democráticos em cada unidade produtiva.

O CiberCom não é uma utopia nem um projeto acabado. É um projeto de código aberto, em evolução coletiva, que parte das condições tecnológicas e organizacionais existentes e de uma análise crítica das tentativas do século XX. A documentação abaixo especifica o modelo com o detalhe necessário para que ele possa ser criticado, simulado e testado.

## Estrutura do modelo

O modelo tem dois níveis interligados e uma camada de verificação.

**Nível macro — o quê produzir.** Decidido pela população, por voto direto, sob a forma de planos de produção com horizonte definido, elaborados por um Corpo Deliberativo Técnico revogável e validados sob a regra do quórum mínimo. O dinheiro é substituído por vales-trabalho pessoais, intransferíveis, não acumuláveis e destruídos no consumo.

**Nível micro — como produzir.** Coordenado pelo Sistema Automatizado de Coleta e Computação de Informação (SACCI), que mantém a matriz insumo-produto da economia em tempo real, decompõe o plano votado em ordens de suprimento e produção, recomputa o plano diante de choques e devolve informação a todos os nós. As decisões operacionais são dos conselhos de trabalhadores de cada unidade; nenhuma alteração de intensidade ou tempo de trabalho ocorre sem o seu consentimento.

**Camada de verificação.** O SACCI verifica por construção fluxos, consumo e coeficientes; regras explícitas tratam da barganha sobre metas, dos produtores únicos, da governança e da privacidade do próprio sistema, e um programa de simulação e experimentos define o que contaria como refutação do modelo.

## Documentos

| Documento | Conteúdo |
|---|---|
| [Democracia Direta Digital](Democracia%20Direta%20Digital.md) | Células, esferas de decisão, atribuição de esferas, corpos técnico e executivo, recall, enquadramento das opções, carga decisória (lei de parâmetros, decisão por exceção, delegação por tema, minipúblicos), segurança do voto |
| [Quórum Mínimo](Quorum%20Minimo.md) | Definição formal do quórum com preferência ordenada, rodadas, dissolução e validação provisória |
| [Planos de Produção](Planos%20de%20Producao.md) | Horizontes, estrutura e decomposição do plano, custo de plano e de equilíbrio, FAA, taxa de dedução d, equação de fechamento, fundo de reserva, replanejamento |
| [Tokens de Valor](tokens-valor.md) | Vales-trabalho: propriedades, remuneração, renda social básica, poupança finalista, cuidado por titularidade, mercado informal |
| [SACCI](SACCI.md) | Arquitetura, coleta, computação, distribuição, plano vinculante e decisão local, resposta a choques |
| [Verificação e Incentivos](Verificacao%20e%20Incentivos.md) | O que o SACCI verifica por construção, o que exige regra, barganha sobre metas, regime de produtores únicos, serviços e qualidade, proteção ao denunciante |
| [Governança e Privacidade do SACCI](Governanca%20e%20Privacidade.md) | Código público, função objetivo votada, implementações independentes, explicabilidade e contestação de ordens, corpo técnico do SACCI, duas contabilidades, chaves separadas |
| [Inovação e Entrada](Inovacao%20e%20Entrada.md) | Orçamento de experimentação, verificação automática, pré-compromisso em vales, esteira de entrada por portões, saída automática, fila por sorteio ponderado, prêmio ao inovador |
| [Cooperativas](Cooperativas.md) | Unidades autogeridas com direito de uso, mandato, integração ao SACCI, resumo técnico de custos, renda e fechamento contábil |
| [Comércio Exterior](Comercio%20Exterior.md) | Monopólio social, custo do importado em horas, moeda estrangeira fora da circulação interna, controles de capital, comércio em tempo de trabalho entre economias afins, limites |
| [Programa de Verificação](Programa%20de%20Verificacao.md) | Simulação com matrizes de insumo-produto reais, simulação de agentes, experimentos em escala reduzida, critérios de refutação |
| [Algoritmos](Algoritmos.md) | Definições operacionais: balanço material (Gauss–Seidel esparso), horas embutidas (Jacobi), programação linear e valores duais, normalização do FAA, custo de equilíbrio, fechamento e d, apuração de votações, sorteios auditáveis, nós críticos, detecção de anomalias, atribuição de esferas, teto de atenção |
| [Parâmetros](Parametros.md) | Tabela de todos os parâmetros votados, símbolos, quem os fixa e valores de referência para simulação |
| [SACCI-Core](SACCI-Core.md) | Especificação do planejamento central: componentes, modelo de dados, APIs, ciclos de execução, requisitos, implementações independentes, simulador |
| [Distribuição](Distribuicao.md) | Pontos de distribuição: retirada com VT, entrega dos Mínimos, serviços, devoluções, poupança, pré-compromisso, agregação e privacidade |
| [VoteSystem](VoteSystem.md) | Especificação do sistema de votação: propriedades, papéis, modelo de dados, fluxos (plano, decisão por exceção, delegação, recall, veto), credenciais, voto presencial |
| [NodeClient](NodeClient.md) | Especificação técnica do software do nó produtivo (Electron + Angular, SQLite, sincronização com o SACCI-Core, simulador embarcado) |
| [Next Steps](Next_steps.md) | Roteiro de desenvolvimento: NodeClient, SACCI-Core, integração e simulação em rede, caminho para produção |
| [Glossário](Glossario.md) | Definições de todos os termos do modelo |
| [Mínimos Cibercomunistas](M%C3%ADnimos%20Cibercomunistas/introducao.md) | Piso material universal garantido in natura (alimentação, moradia, saúde, educação, acesso digital, cultura, mobilidade): bases jurídicas, métricas e padrões por domínio; entra no plano como restrição de nível de atendimento mínimo |

## Terminologia

O projeto evita as palavras "preço" e "salário" para o que ocorre dentro do modelo, porque não há mercado nem venda de força de trabalho: fala-se em **custo social** (custo de plano P*, custo de equilíbrio P) dos bens de consumo, e em **renda** em vales-trabalho. "Preço" e "salário" aparecem apenas quando se descreve o capitalismo ou o comércio exterior.

## Notação

| Símbolo | Significado |
|---|---|
| hᵢ | horas trabalhadas pelo trabalhador i no período |
| mᵢ | multiplicador de renda do trabalhador i (1 por padrão; prêmios por penosidade, escassez, criticidade) |
| d | taxa de dedução social (fração das horas destinada aos fundos comuns) |
| B | renda social básica por cidadão, em VT |
| N | número de cidadãos |
| S, ΔS | saldo de poupança finalista; sua variação líquida no período |
| Hⱼ | horas embutidas (diretas + indiretas) na unidade do bem j |
| Qⱼ | quantidade planejada do bem j para consumo individual |
| FAAⱼ | Fator de Ajuste Ambiental do bem j (normalizado) |
| P*ⱼ | custo de plano do bem j = FAAⱼ · Hⱼ |
| Pⱼ | custo de equilíbrio do bem j |
| A | matriz de coeficientes técnicos (insumo-produto) |
| LM | Lista de Materiais de um item |

## Referências teóricas

Marx, *Crítica do Programa de Gotha* e *O Capital*, Livro II, cap. 18; Cockshott e Cottrell, *Towards a New Socialism* (1993), "Calculation, Complexity and Planning" (1993), "Information and Economics: A Critique of Hayek" (1997), "Economic planning, computers and labor values" (1999); Cockshott, "Mises, Kantorovich and Economic Computation" (2007) e "Big Data and Super-Computers" (2017); Kantorovich (1939); Leontief (1941); Kornai, *Economics of Shortage* (1980); Dapprich (2023); Saros (2014). A pasta `fontes teoricas` do projeto reúne os textos.

## Implementação

O software é desenvolvido neste repositório: `node-client/` ([NodeClient](NodeClient.md)), `vote-system/` ([VoteSystem](VoteSystem.md)), `sacci-core/` ([SACCI-Core](SACCI-Core.md)) e o ponto de distribuição ([Distribuição](Distribuicao.md)). Os algoritmos que toda implementação deve reproduzir estão em [Algoritmos](Algoritmos.md); o roteiro em [Next Steps](Next_steps.md). Toda implementação deve satisfazer os requisitos de [Governança e Privacidade](Governanca%20e%20Privacidade.md): dados individuais permanecem no nó e no dispositivo do titular; o sistema central recebe totais com compromissos criptográficos.

## Licença

Ver [LICENSE](LICENSE).
