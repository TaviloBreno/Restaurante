# Regras de Negócio — Sistema de Restaurante

## RN-001: Pizza Meio-a-Meio

**Descrição**: Um cliente pode escolher até 2 sabores para uma mesma pizza.

**Regras:**
- O tamanho mínimo para meio-a-meio é **Média** (Broto não permite).
- O preço é calculado como **o maior preço entre os dois sabores** (regra comum no mercado).
- Se um dos sabores tiver adicional, o adicional é cobrado **proporcional** (50%).
- Meio-a-meio **não** pode combinar com combo.

**Exemplo:**
- Pizza Grande Calabresa (R$ 50) + Grande Marguerita (R$ 45) → R$ 50.

**Impacto no domínio:**
- `Pizza` precisa de `SaborPrincipal` + `SaborSecundario?`
- `PrecoPizza` calculado por política `CalcularPrecoMeioAMeio`.

---

## RN-002: Adicionais

**Descrição**: Itens extras vinculados a produtos.

**Regras:**
- Adicional só pode ser vinculado a Pizza ou Sanduíche.
- Bebidas **não** aceitam adicionais.
- Cada adicional tem preço próprio.
- Limite de **5 adicionais** por item (evitar abuso).
- Adicional duplicado não é permitido (ex.: dois "bacon extra").

**Exemplos de adicionais:**
- Pizza: borda catupiry (+R$ 8), queijo extra (+R$ 5), bacon (+R$ 6).
- Sanduíche: ovo (+R$ 3), cheddar (+R$ 4).

---

## RN-003: Combo

**Descrição**: Conjunto promocional com preço especial.

**Regras:**
- Combo é composto por **1 item principal + 1 bebida** no mínimo.
- Itens de combo **não aceitam** adicionais.
- Desconto padrão: **15%** sobre a soma dos itens.
- Combo **não pode ser cancelado** após `PedidoConfirmado`.
- Mesmo combo não pode ser aplicado 2x no mesmo pedido.

**Exemplo:**
- Pizza Média (R$ 40) + Refrigerante Lata (R$ 8) → R$ 48 - 15% = **R$ 40,80**.

---

## RN-004: Tamanhos de Pizza

| Tamanho | Fatias | Diâmetro | Aceita Meio-a-Meio? |
|---------|--------|----------|---------------------|
| Broto | 4 | 20cm | ❌ |
| Média | 6 | 30cm | ✅ |
| Grande | 8 | 35cm | ✅ |
| Família | 12 | 45cm | ✅ |

---

## RN-005: Taxa de Entrega

- Fixa por bairro (tabela `Bairro → Valor`).
- Pedido mínimo para entrega: **R$ 30**.
- Entrega grátis para pedidos acima de **R$ 100**.
- Cliente retirada no local: sem taxa.

---

## RN-006: Estados do Pedido

- Pedido só pode ser **cancelado** antes de `PreparoIniciado`.
- Após `PedidoEntregue`, não pode haver alteração.
- Se `PagamentoRecusado`, pedido volta para `AguardandoPagamento` (máx. 3 tentativas).

---

## RN-007: Estoque

- Baixa de insumos ocorre **ao confirmar o preparo** (não ao criar pedido).
- Se insumo < 5 unidades, emitir `EstoqueBaixo`.
- Produto com estoque zerado fica automaticamente **indisponível** no cardápio.

---

## RN-008: Preço e Arredondamento

- Todos os valores monetários usam **decimal(10,2)**.
- Arredondamento: **MidpointRounding.AwayFromZero**.
- Nunca usar `double` ou `float` para dinheiro.

---

## Matriz de Rastreabilidade (User Story × Regra × Evento)

| User Story | Regra de Negócio | Evento de Domínio |
|------------|------------------|-------------------|
| US-02 (Adicionar item) | RN-001, RN-002 | ItemAdicionadoAoCarrinho |
| US-03 (Endereço) | RN-005 | EnderecoInformado |
| US-05 (Pagamento) | RN-006 | PagamentoAprovado |
| US-08 (Combo) | RN-003 | ComboAplicado |
| US-12 (Cancelamento) | RN-006 | PedidoCancelado |