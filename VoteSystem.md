# Especificações Técnicas: Sistema de Votação (vote-system)

O sistema de votação implementa a [Democracia Direta Digital](Democracia%20Direta%20Digital.md): publicação de opções com simulação, minipúblicos, votação com preferência ordenada, apuração pelo [Quórum Mínimo](Quorum%20Minimo.md), decisão por exceção, delegação por tema, recall, iniciativas e contestações. A escolha de tecnologia é livre, sujeita aos requisitos abaixo; os algoritmos de apuração e sorteio estão em [Algoritmos](Algoritmos.md) §6–§7.

## 1. Propriedades exigidas

| Propriedade | Definição | Verificação |
|---|---|---|
| **Elegibilidade** | Só cidadãos da esfera votam, uma vez cada (o último voto conta) | Credencial de voto emitida pela célula |
| **Verificabilidade individual** | O votante confirma que seu voto foi registrado como emitido | Recibo de inclusão que não revela o conteúdo |
| **Verificabilidade universal** | Qualquer pessoa reconta a partir dos dados publicados | Cédulas cifradas publicadas + prova de apuração |
| **Sigilo** | Ninguém, inclusive o sistema, liga voto a votante | Cifragem com mistura ou agregação homomórfica |
| **Sem recibo** | O votante não pode provar a terceiros como votou | Revoto; ausência de prova de conteúdo |
| **Resistência à coerção** | Votar sob coação não fixa o resultado | Revoto até o fechamento; alternativa presencial |
| **Não vinculação** | A chave de voto não é ligável às de trabalho e consumo | Emissão separada; sem identificador comum |
| **Contagem redundante** | Apuração executada por todas as implementações independentes | Comparador ([SACCI-Core](SACCI-Core.md) §6) |

O protocolo criptográfico concreto deve ser de código aberto, auditado e escolhido por decisão por exceção na esfera nacional; a especificação fixa as propriedades, não o protocolo.

## 2. Papéis

- **Célula**: emite e revoga credenciais de voto (uma por cidadão), mantém o eleitorado E da sua esfera, opera a votação presencial em papel.
- **CDT**: publica opções com simulação.
- **Grupo proponente**: publica opção popular (com assinaturas) e, opcionalmente, sua própria execução da simulação.
- **Minipúblico**: sorteado por esfera para cada votação de plano/lei de parâmetros; publica recomendação.
- **Implementações do SACCI**: executam simulações e apuração.
- **Cidadão**: vota, revota, delega, contesta, assina iniciativas.

## 3. Modelo de dados

- **Ballot**: `Id`, `SphereId`, `Type` (Plano | LeiParametros | Lei | Contestação | Recall | Dissolução | Veto | Iniciativa), `Options[]`, `OpensAt`, `ClosesAt`, `Round` (1..R), `Parameters` (q, a, a', R), `Status`.
- **Option**: `Id`, `Origin` (CDT | Popular), `Text`, `Simulations[] {ImplementationId, PlanVersionHash, Summary}`, `Recommendation` (do minipúblico), `Signatures` (para opção popular).
- **Vote** (cifrado): `BallotId`, `Ranking[]` (ordenação parcial), `Timestamp` — publicado cifrado; o último por credencial prevalece na apuração.
- **Delegation**: `DelegatorCredential`, `DelegateId` (público), `Topic`, `ValidFrom`, `RevokedAt` — a lista de delegações por delegado é pública (quantidade e temas); o delegante não é revelado.
- **Petition**: `Type` (Iniciativa | OpçãoPopular | Recall | Contestação), `Target`, `Signatures` (credenciais, verificadas sem revelar identidade além da unicidade), `Threshold`, `Deadline`.
- **DefaultDecision** (decisão por exceção): `ProposalId`, `PublishedAt`, `ContestDeadline`, `ContestSignatures`, `Status` (EmVigorPorPadrão | Contestada | Votada).
- **Panel** (minipúblico): `BallotId`, `SphereId`, `Seed`, `Members` (credenciais sorteadas; nomes públicos apenas com consentimento), `Recommendation`, `HoursPaid`.
- **Tally**: `BallotId`, `Round`, `PairwiseMatrix`, `Participation`, `Adhesion`, `Result` (Validada(o) | SemDecisão | ProvisóriaMaisPróxima), `ImplementationHashes[]`, `Proof`.

## 4. Fluxos

### 4.1 Votação de plano / lei de parâmetros
```
1. CDT publica opções (≥ 2) + simulações por todas as implementações        [T−15 d]
2. janela para opções populares (assinaturas ≥ limiar) + suas simulações      [T−15 … T−8 d]
3. sorteio do minipúblico (semente pública); deliberação; recomendação        [T−12 … T−2 d]
4. votação aberta: preferência ordenada; revoto permitido; papel nas células  [T … T+T_r]
5. consolidação do papel; apuração por todas as implementações; publicação    [T+T_r]
6. se SemDecisão: CDT revisa opções → rodada seguinte; após R rodadas, dissolução
```
Durante a ausência de decisão sobre um novo plano, o plano vigente continua ([Quórum Mínimo](Quorum%20Minimo.md) §4).

### 4.2 Decisão por exceção
```
1. proposta publicada com simulação e ContestDeadline = agora + T_c
2. cidadãos da esfera podem assinar contestação; se assinaturas ≥ f_c · |E| antes do prazo → Ballot (Tipo Contestação)
3. senão, entra em vigor no prazo; registro público
```

### 4.3 Delegação
- O cidadão delega por tema a uma pessoa identificada; revogação com efeito imediato; teto D_max por delegado, verificado na aceitação.
- Na apuração, cada voto não emitido pelo delegante em cédula do tema é substituído pelo voto do delegado (não transitivo). Cédulas de plano, lei de parâmetros, recall e dissolução ignoram delegações.
- Os votos dos delegados são públicos por cédula.

### 4.4 Recall e dissolução
- Recall: petição com assinaturas ≥ limiar → cédula de recall na esfera que escolheu o membro; maioria simples com quórum q.
- Dissolução automática: disparada pelo sistema após R rodadas sem decisão; convoca eleição/sorteio do novo CDT na esfera.

### 4.5 Veto na esteira de entrada
- Veto fundamentado a uma proposta de experimentação ([Inovação e Entrada](Inovacao%20e%20Entrada.md) §2) é publicado como `DefaultDecision` invertida: o veto só se sustenta se votado; silêncio mantém o direito de uso.

## 5. Credenciais e chaves

- A célula emite a credencial de voto após verificação presencial da pessoa; a credencial é uma chave de voto sem ligação com as chaves de trabalho e de consumo (emitidas em processos separados, sem identificador comum armazenado).
- Revogação (mudança de célula, óbito, perda): lista de revogação pública por esfera, sem identidade.
- O eleitorado |E| de cada esfera é o número de credenciais válidas, publicado.

## 6. Voto presencial

Cada célula oferece, para planos e leis de parâmetros, cédula em papel com a mesma preferência ordenada. A apuração presencial é feita publicamente na célula, registrada e consolidada com a eletrônica; um cidadão que votou em papel tem a credencial marcada para aquela cédula (o voto em papel é o último).

## 7. Teto de atenção

O sistema calcula, com o SACCI, as votações esperadas por cidadão por mês ([Algoritmos](Algoritmos.md) §11) e recusa a publicação de um plano cuja carga prevista exceda V_max.

## 8. Publicação

Por cédula: opções, simulações, recomendações, matriz par a par, participação, adesão, resultado, hashes das implementações, prova de apuração, cédulas cifradas. Por esfera: eleitorado, delegações por delegado, decisões por exceção vigentes e contestadas, petições abertas.
