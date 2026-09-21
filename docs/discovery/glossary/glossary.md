# Glossário Ubíquo — Sistema de Restaurante

## Termos Gerais

| Termo | Definição | Contexto | Sinônimos Proibidos |
|-------|-----------|----------|---------------------|
| **Produto** | Item vendável do cardápio. Pode ser Pizza, Sanduíche ou Bebida. | Catálogo | "Item", "Mercadoria" |
| **Item do Cardápio** | Produto disponível para venda em determinado momento. | Catálogo | — |
| **Categoria** | Agrupamento lógico de produtos (Pizzas, Sanduíches, Bebidas, Sucos). | Catálogo | "Tipo", "Grupo" |
| **Adicional** | Item extra vinculado a um produto (borda, queijo extra, bacon). | Catálogo | "Extra", "Complemento" |
| **Combo** | Conjunto promocional de produtos com preço especial. | Catálogo | "Promoção", "Kit" |

## Produtos

| Termo | Definição | Regras |
|-------|-----------|--------|
| **Pizza** | Produto circular dividido em sabores. Tamanhos: Broto, Média, Grande, Família. | Aceita meio-a-meio, adicionais e borda recheada |
| **Sanduíche** | Produto montado em pão. Tamanhos: Único. | Aceita adicionais, não aceita meio-a-meio |
| **Refrigerante** | Bebida gaseificada industrializada. | Vendido em lata (350ml) ou garrafa (600ml/2L) |
| **Suco** | Bebida natural de fruta. | Sabores: laranja, limão, abacaxi, maracujá. Sem adicionais |

## Pedido

| Termo | Definição |
|-------|-----------|
| **Pedido** | Solicitação de compra feita por um cliente. Agregado raiz. |
| **Item do Pedido** | Cada linha do pedido (produto + quantidade + adicionais). |
| **Carrinho** | Estado inicial do Pedido, antes da confirmação. |
| **Subtotal** | Soma dos itens sem taxas. |
| **Taxa de Entrega** | Valor cobrado pela entrega (fixo por bairro). |
| **Total** | Subtotal + taxa de entrega - descontos. |

## Estados do Pedido

| Estado | Significado | Transição |
|--------|-------------|-----------|
| **Rascunho** | Carrinho em montagem | → AguardandoPagamento |
| **AguardandoPagamento** | Cliente confirmou, aguardando PIX/cartão | → Pago, Cancelado |
| **Pago** | Pagamento aprovado | → EmPreparo |
| **EmPreparo** | Cozinha preparando | → Pronto |
| **Pronto** | Aguardando entregador | → EmRota |
| **EmRota** | Entregador a caminho | → Entregue |
| **Entregue** | Pedido finalizado | (final) |
| **Cancelado** | Pedido cancelado | (final) |

## Atores

| Termo | Definição |
|-------|-----------|
| **Cliente** | Pessoa que faz o pedido (identificado por CPF/telefone). |
| **Atendente** | Funcionário que registra pedidos presenciais/telefone. |
| **Cozinheiro** | Funcionário responsável pela produção. |
| **Entregador** | Funcionário/motoboy que realiza a entrega. |
| **Administrador** | Gerencia cardápio, preços, usuários e relatórios. |

## Termos Técnicos (evitar no domínio)

| ❌ Evitar | ✅ Usar |
|----------|--------|
| "Item" (ambíguo) | "ItemPedido" ou "ItemCardapio" |
| "Objeto" | Nome específico do agregado |
| "Flag" | Status nomeado |
| "Dados" | Coleção nomeada |