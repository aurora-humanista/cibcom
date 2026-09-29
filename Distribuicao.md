# Distribuição: pontos de distribuição e consumo final

O **ponto de distribuição (PD)** é o nó onde a população retira bens e serviços de consumo individual com vales-trabalho, onde os bens e serviços dos Mínimos são entregues gratuitamente, e onde o consumo final é registrado para o SACCI. É a única interface entre a contabilidade dos nós e a contabilidade das pessoas, e por isso tem regras próprias de privacidade.

Referências: [Tokens de Valor](tokens-valor.md), [Planos de Produção](Planos%20de%20Producao.md) §5, [Governança e Privacidade](Governanca%20e%20Privacidade.md) §2.

## 1. Funções

1. **Retirada com VT**: apresenta o catálogo com o custo de equilíbrio corrente de cada bem; ao retirar, destrói P_j VT da conta do titular (chave de consumo).
2. **Entrega dos Mínimos**: bens e serviços dos [Mínimos Cibercomunistas](M%C3%ADnimos%20Cibercomunistas/introducao.md) são entregues **sem custo em VT**, contra o direito da pessoa (por exemplo, a cesta alimentar de referência por período); a entrega é registrada como consumo coletivo, agregada.
3. **Serviços de consumo**: agendamento, execução e **confirmação pelo destinatário** (a destruição do VT é a confirmação); avaliação.
4. **Estoque e giro**: recebe `SupplyOrder`s dos nós produtores; registra estoques; o giro observado alimenta o custo de equilíbrio.
5. **Devoluções e garantia**: registro de devolução (VT restituídos) e de acionamento de garantia; alimentam o sinal de qualidade do produtor.
6. **Poupança finalista**: operações de reserva de saldo com prazo, na conta do titular (a conta vive no dispositivo do titular; o PD apenas executa).
7. **Pré-compromisso**: registro pseudônimo de vales comprometidos em propostas do catálogo ([Inovação e Entrada](Inovacao%20e%20Entrada.md) §3).

## 2. Modelo de dados

- **[PD] CatalogItem**: `ItemCode`, `EquilibriumCost P`, `PlanCost P*`, `Stock`, `MinimumEntitlement` (se pertence aos Mínimos).
- **[PD] DistributionRecord** (agregado): `ItemCode`, `Period`, `Region`, `Quantity`, `VTDestroyed`, `Returns`, `Assessments` (distribuição de notas) — o que vai ao SACCI.
- **[pessoa] Account**: no dispositivo do titular: `Balance`, `SavingsLots[] {Amount, ExpiresAt}`, `Commitments[] {ProposalId, Amount, ExpiresAt}`, `History` (local).
- **[pessoa] Entitlements**: direitos dos Mínimos e de cuidado por titularidade, emitidos pela célula, verificáveis pelo PD sem revelar identidade além do necessário para evitar duplicidade (credencial anônima com prova de unicidade por período).

## 3. Fluxo de retirada

```
1. titular apresenta credencial de consumo (chave de consumo)
2. PD verifica validade e, para item dos Mínimos, o direito no período (credencial anônima)
3. PD calcula P_j × quantidade; conta do titular destrói os VT (assinatura do titular)
4. PD registra saída de estoque e incrementa o agregado do período
5. ao fechar o período, PD envia ao SACCI: agregados por item/região + compromisso criptográfico
   de que a soma das destruições individuais = VT destruídos agregados
```
O PD **não guarda** o histórico individual; guarda apenas o agregado e os compromissos. Granularidade mínima de agregação conforme [Parâmetros](Parametros.md) (região com ≥ 100 pessoas).

## 4. Custos no PD

- O custo exibido é o custo de equilíbrio corrente P_j; o custo de plano P*_j é exibido ao lado, com a razão, para transparência.
- Bens dos Mínimos têm custo em VT zero para o titular do direito; retiradas acima do direito são cobradas a P_j.
- O PD não pratica descontos, promoções nem custos diferenciados por pessoa: o custo é o mesmo para todos.

## 5. Operação offline e partição

O PD opera com fila local; retiradas em partição são registradas e sincronizadas depois. A conta do titular, no dispositivo, é a fonte de verdade do saldo; conflitos (dupla destruição) são resolvidos a favor do menor saldo e sinalizados.

## 6. Indicadores enviados ao SACCI

Por item, período e região: quantidade retirada, VT destruídos, estoque final, dias de ruptura (estoque zero), devoluções, distribuição de avaliações, entregas de Mínimos versus direitos emitidos (cobertura). São os dados do custo de equilíbrio, do sinal de qualidade, do diagnóstico de mercado informal (custo no teto + ruptura) e do monitoramento dos Mínimos (cobertura por domínio e região).

## 7. Serviços de consumo individual

Serviços (reparos, cuidados padronizáveis, cultura, transporte além do mínimo) são catalogados com custo de plano em horas embutidas e custo de equilíbrio; o agendamento é feito no PD ou diretamente no nó prestador; a confirmação do destinatário destrói os VT e encerra o `ServiceRecord` do prestador.
