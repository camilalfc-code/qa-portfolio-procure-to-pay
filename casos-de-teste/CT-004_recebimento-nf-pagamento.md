# CT-004 — Recebimento, Nota Fiscal e Pagamento

## Contexto

Após a emissão e confirmação da Ordem de Compra, o fornecedor realiza a entrega no almoxarifado. O almoxarifado confere os itens e registra a entrada no sistema **somente quando a entrega está completa e correta**. A Nota Fiscal é encaminhada ao financeiro, que realiza a liquidação e o pagamento em até 30 dias.

**Fluxo completo:**
```
Entrega no almoxarifado → Conferência → Registro de entrada no sistema → 
NF encaminhada ao financeiro → Liquidação → Pagamento
```

**Perfis de usuário:**
- `ALMOXARIFADO` — Recebe, confere e registra a entrada de mercadorias
- `FINANCEIRO` — Recebe a NF, realiza a liquidação e o pagamento
- `COMPRAS` — Acompanha o processo e aciona fornecedor em caso de divergência

---

## Casos de Teste — Recebimento no Almoxarifado

---

### CT-004-01 — Registro de entrada de mercadoria completa e correta

| Campo | Valor |
|---|---|
| **ID** | CT-004-01 |
| **Módulo** | Recebimento — Almoxarifado |
| **Tipo** | Funcional — Fluxo Principal |
| **Prioridade** | Alta |
| **Pré-condição** | PO confirmada / Entrega realizada pelo fornecedor / Usuário com perfil ALMOXARIFADO autenticado |

**Passos:**
1. Acessar o módulo de entrada de mercadorias
2. Localizar a PO vinculada à entrega
3. Conferir os itens recebidos: quantidade, descrição e condição
4. Registrar a entrada confirmando que tudo está correto
5. Clicar em "Confirmar recebimento"

**Resultado esperado:**
- Entrada registrada no sistema vinculada à PO
- Status da PO atualizado para "Entregue — Aguardando NF"
- Notificação enviada ao financeiro informando que a entrega foi confirmada
- Data e hora do recebimento registradas no histórico

---

### CT-004-02 — Tentativa de registrar entrada com quantidade divergente da PO

| Campo | Valor |
|---|---|
| **ID** | CT-004-02 |
| **Módulo** | Recebimento — Almoxarifado |
| **Tipo** | Validação — Regra de Negócio |
| **Prioridade** | Alta |
| **Pré-condição** | PO com 500 unidades / Entrega de 500 unidades mas almoxarifado tenta registrar 600 |

**Passos:**
1. Acessar o módulo de entrada de mercadorias
2. Localizar a PO vinculada
3. Informar quantidade recebida superior à quantidade da PO (ex: 600 de 500)
4. Tentar confirmar o recebimento

**Resultado esperado:**
- Sistema bloqueia o registro
- Mensagem: "Quantidade informada superior à quantidade da ordem de compra. Verifique os dados."
- Entrada não é registrada

---

### CT-004-03 — Entrega parcial com NF faturada na quantidade entregue

| Campo | Valor |
|---|---|
| **ID** | CT-004-03 |
| **Módulo** | Recebimento — Almoxarifado |
| **Tipo** | Funcional — Fluxo Alternativo |
| **Prioridade** | Alta |
| **Pré-condição** | PO com 500 unidades / Fornecedor entrega 300 com NF de 300 unidades |

**Passos:**
1. Acessar o módulo de entrada de mercadorias
2. Localizar a PO vinculada
3. Registrar entrada de 300 unidades
4. Informar que a entrega é parcial
5. Vincular NF referente às 300 unidades entregues
6. Confirmar recebimento parcial

**Resultado esperado:**
- Entrada de 300 unidades registrada
- Status da PO atualizado para "Entrega parcial — Saldo pendente: 200 unidades"
- NF das 300 unidades encaminhada ao financeiro para liquidação parcial
- Sistema mantém saldo de 200 unidades em aberto para entrega futura

---

### CT-004-04 — Entrega parcial com NF faturada na quantidade total da PO

| Campo | Valor |
|---|---|
| **ID** | CT-004-04 |
| **Módulo** | Recebimento — Almoxarifado |
| **Tipo** | Exceção — Regra de Negócio |
| **Prioridade** | Alta |
| **Pré-condição** | PO com 500 unidades / Fornecedor entrega 300 mas envia NF de 500 unidades |

**Passos:**
1. Almoxarifado recebe 300 unidades
2. NF apresenta quantidade total de 500 unidades
3. Almoxarifado tenta registrar a entrada com a NF divergente

**Resultado esperado:**
- Sistema bloqueia o registro ou emite alerta de divergência
- Mensagem: "A quantidade da NF diverge da quantidade entregue. A NF ficará retida até a entrega completa."
- NF não é encaminhada ao financeiro
- Status: "NF retida — Aguardando entrega completa"
- Compras é notificado para acionar o fornecedor

---

## Casos de Teste — Nota Fiscal

---

### CT-004-05 — NF com valor correto — encaminhamento para liquidação

| Campo | Valor |
|---|---|
| **ID** | CT-004-05 |
| **Módulo** | Nota Fiscal |
| **Tipo** | Funcional — Fluxo Principal |
| **Prioridade** | Alta |
| **Pré-condição** | Recebimento confirmado / NF com valor igual ao da PO |

**Passos:**
1. Financeiro acessa as NFs pendentes de liquidação
2. Localizar a NF vinculada à PO
3. Conferir: valor, CNPJ do fornecedor, descrição do item e data de emissão
4. Confirmar que o valor da NF bate com o valor da PO
5. Clicar em "Liquidar"

**Resultado esperado:**
- NF liquidada com data de liquidação registrada
- Status atualizado para "Liquidada — Aguardando pagamento"
- Prazo de pagamento calculado automaticamente (ex: 30 dias a partir da liquidação)
- Histórico atualizado

---

### CT-004-06 — NF com valor diferente da PO

| Campo | Valor |
|---|---|
| **ID** | CT-004-06 |
| **Módulo** | Nota Fiscal |
| **Tipo** | Exceção — Divergência de Valor |
| **Prioridade** | Alta |
| **Pré-condição** | NF recebida com valor diferente do aprovado na PO |

**Passos:**
1. Financeiro acessa NF pendente
2. Identifica que o valor da NF é diferente do valor da PO
3. Registra a divergência no sistema
4. Encaminha contestação ao fornecedor

**Resultado esperado:**
- Sistema registra a divergência vinculada à NF e à PO
- Status da NF atualizado para "Contestada — Aguardando correção do fornecedor"
- Notificação enviada ao fornecedor solicitando correção ou carta de desconto
- NF não é liquidada até resolução da divergência
- Histórico completo da contestação disponível para auditoria

---

### CT-004-07 — NF com CNPJ diferente do fornecedor da PO

| Campo | Valor |
|---|---|
| **ID** | CT-004-07 |
| **Módulo** | Nota Fiscal |
| **Tipo** | Exceção — Dado Inválido |
| **Prioridade** | Alta |

**Passos:**
1. Financeiro tenta liquidar NF
2. Sistema identifica que o CNPJ da NF é diferente do CNPJ do fornecedor cadastrado na PO

**Resultado esperado:**
- Sistema bloqueia a liquidação
- Mensagem: "CNPJ da NF diverge do fornecedor da ordem de compra. Verifique os dados."
- NF não é liquidada

---

## Casos de Teste — Pagamento

---

### CT-004-08 — Pagamento realizado dentro do prazo

| Campo | Valor |
|---|---|
| **ID** | CT-004-08 |
| **Módulo** | Pagamento |
| **Tipo** | Funcional — Fluxo Principal |
| **Prioridade** | Alta |
| **Pré-condição** | NF liquidada / Prazo de pagamento dentro do vencimento |

**Passos:**
1. Financeiro acessa a lista de NFs liquidadas com pagamento pendente
2. Localizar NF com vencimento próximo
3. Processar o pagamento ao fornecedor
4. Registrar o comprovante de pagamento no sistema

**Resultado esperado:**
- Pagamento registrado com data, valor e comprovante vinculado
- Status da PO atualizado para "Concluída — Paga"
- Histórico completo do ciclo disponível: solicitação → cotação → PO → entrega → NF → pagamento

---

### CT-004-09 — Tentativa de pagar NF ainda não liquidada

| Campo | Valor |
|---|---|
| **ID** | CT-004-09 |
| **Módulo** | Pagamento |
| **Tipo** | Validação — Regra de Negócio |
| **Prioridade** | Alta |

**Passos:**
1. Financeiro tenta processar pagamento de NF com status "Contestada" ou "Pendente"

**Resultado esperado:**
- Sistema bloqueia o pagamento
- Mensagem: "Esta NF não pode ser paga pois ainda não foi liquidada."

---

### CT-004-10 — Pagamento com valor diferente da NF liquidada

| Campo | Valor |
|---|---|
| **ID** | CT-004-10 |
| **Módulo** | Pagamento |
| **Tipo** | Validação — Valor |
| **Prioridade** | Alta |

**Passos:**
1. Financeiro acessa NF liquidada no valor de R$ 5.000,00
2. Tenta registrar pagamento de R$ 4.500,00

**Resultado esperado:**
- Sistema alerta sobre a divergência de valor
- Mensagem: "O valor do pagamento difere do valor liquidado. Confirme antes de prosseguir."
- Pagamento só é processado após confirmação explícita do usuário

---

## Resumo dos Casos de Teste

| ID | Descrição | Tipo | Prioridade |
|---|---|---|---|
| CT-004-01 | Registro de entrada completa e correta | Fluxo principal | Alta |
| CT-004-02 | Quantidade recebida superior à PO | Validação | Alta |
| CT-004-03 | Entrega parcial com NF parcial | Fluxo alternativo | Alta |
| CT-004-04 | Entrega parcial com NF total | Exceção | Alta |
| CT-004-05 | NF com valor correto — liquidação | Fluxo principal | Alta |
| CT-004-06 | NF com valor divergente da PO | Exceção | Alta |
| CT-004-07 | NF com CNPJ diferente do fornecedor | Dado inválido | Alta |
| CT-004-08 | Pagamento dentro do prazo | Fluxo principal | Alta |
| CT-004-09 | Pagamento de NF não liquidada | Validação | Alta |
| CT-004-10 | Pagamento com valor divergente | Validação | Alta |
