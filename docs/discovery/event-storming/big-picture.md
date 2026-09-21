# Event Storming — Big Picture

## Fluxo Principal: Pedido de Delivery

### Fase 1 — Descoberta do Cardápio
🟨 Cliente → 🟦 ConsultarCardapio → 🟩 Cardápio → 🟧 CardapioConsultado

### Fase 2 — Montagem do Pedido
🟨 Cliente → 🟦 AdicionarItemAoCarrinho → 🟧 ItemAdicionadoAoCarrinho
🟨 Cliente → 🟦 RemoverItemDoCarrinho → 🟧 ItemRemovidoDoCarrinho
🟨 Cliente → 🟦 InformarEnderecoEntrega → 🟧 EnderecoInformado
🟨 Cliente → 🟦 ConfirmarPedido → 🟧 PedidoCriado

### Fase 3 — Pagamento
🟦 ProcessarPagamento → 🟧 PagamentoIniciado
⬜ GatewayPagamento → 🟧 PagamentoAprovado | 🟧 PagamentoRecusado
🟪 Política: Quando PagamentoAprovado, então iniciar produção

### Fase 4 — Produção
🟨 Cozinheiro → 🟦 IniciarPreparo → 🟧 PreparoIniciado
🟨 Cozinheiro → 🟦 MarcarComoPronto → 🟧 PedidoPronto
🟪 Política: Quando PedidoPronto, então acionar entregador

### Fase 5 — Entrega
🟨 Entregador → 🟦 AceitarEntrega → 🟧 EntregaAceita
🟨 Entregador → 🟦 IniciarRota → 🟧 EntregaIniciada
🟨 Entregador → 🟦 ConfirmarEntrega → 🟧 PedidoEntregue

### Fase 6 — Pós-venda
🟪 Política: Quando PedidoEntregue, então solicitar avaliação
🟨 Cliente → 🟦 AvaliarPedido → 🟧 PedidoAvaliado