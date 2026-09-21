# Event Storming — Process Level

## Bounded Context: Catálogo

### Comandos → Eventos
| Comando | Agregado | Evento | Regra |
|---------|----------|--------|-------|
| CriarProduto | Produto | ProdutoCriado | Nome único por categoria |
| AlterarPreco | Produto | PrecoAlterado | Preço > 0 |
| DesativarProduto | Produto | ProdutoDesativado | Não pode ter pedidos em aberto |
| CriarCategoria | Categoria | CategoriaCriada | Nome único |
| VincularAdicional | Produto | AdicionalVinculado | Adicional pertence ao mesmo contexto |

### Read Models
- 🟩 CardápioPorCategoria
- 🟩 ProdutoComAdicionais
- 🟩 ProdutosIndisponiveis

---

## Bounded Context: Vendas

### Comandos → Eventos
| Comando | Agregado | Evento | Regra |
|---------|----------|--------|-------|
| CriarPedido | Pedido | PedidoCriado | Mínimo 1 item |
| AdicionarItem | Pedido | ItemAdicionado | Pedido em status Rascunho |
| AplicarCombo | Pedido | ComboAplicado | Itens compatíveis com combo |
| ConfirmarPedido | Pedido | PedidoConfirmado | Endereço + pagamento definidos |
| CancelarPedido | Pedido | PedidoCancelado | Somente antes de PreparoIniciado |

### Read Models
- 🟩 CarrinhoAtual
- 🟩 PedidosEmAberto
- 🟩 HistoricoDoCliente

---

## Bounded Context: Pagamento

| Comando | Agregado | Evento |
|---------|----------|--------|
| ProcessarPagamento | Transacao | PagamentoIniciado |
| ConfirmarPagamento | Transacao | PagamentoAprovado |
| RejeitarPagamento | Transacao | PagamentoRecusado |
| EstornarPagamento | Transacao | PagamentoEstornado |

---

## Bounded Context: Entrega

| Comando | Agregado | Evento |
|---------|----------|--------|
| AtribuirEntregador | Entrega | EntregadorAtribuido |
| IniciarRota | Entrega | EntregaIniciada |
| RegistrarChegada | Entrega | EntregadorChegou |
| ConfirmarEntrega | Entrega | PedidoEntregue |

---

## Bounded Context: Estoque

| Comando | Agregado | Evento |
|---------|----------|--------|
| BaixarEstoque | Insumo | EstoqueBaixado |
| ReporEstoque | Insumo | EstoqueReposto |
| MarcarIndisponivel | Insumo | InsumoIndisponivel |

---

## Bounded Context: Identidade

| Comando | Agregado | Evento |
|---------|----------|--------|
| RegistrarUsuario | Usuario | UsuarioRegistrado |
| Autenticar | Usuario | UsuarioAutenticado |
| AtribuirRole | Usuario | RoleAtribuida |

---

## Hotspots (Pontos de Atenção) 🔥

1. **Pizza meio-a-meio**: como modelar? Produto composto? ItemPedido com dois sabores?
2. **Combos dinâmicos**: preço do combo é soma dos itens ou valor fixo?
3. **Cancelamento após produção**: como reverter insumos no estoque?
4. **Entrega sem entregador disponível**: fila? Terceirização?
5. **Pagamento na entrega (dinheiro)**: quando emitir `PagamentoAprovado`?