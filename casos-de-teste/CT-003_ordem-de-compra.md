# CT-003 — Ordem de Compra (Purchase Order)

## Contexto

Após aprovação da cotação pelo gestor financeiro, a equipe de compras emite a **Ordem de Compra (PO — Purchase Order)** para formalizar a aquisição com o fornecedor selecionado. A PO estabelece o item, quantidade, valor acordado, prazo de entrega e condições de pagamento.

O fornecedor tem um prazo para confirmar o recebimento da PO. Após confirmação, inicia-se o prazo de entrega. O pagamento é realizado em até 30 dias após o recebimento da Nota Fiscal.

**Perfis de usuário:**
- `COMPRAS` — Equipe de compras (emite a PO)
- `APROVADOR` — Gestor financeiro (aprovou a cotação na etapa anterior)
- `FORNECEDOR` — Recebe e confirma a PO
- `FINANCEIRO` — Responsável pelo pagamento

---

## Casos de Teste — Emissão da Ordem de Compra

---

### CT-003-01 — Emissão de PO válida com todos os dados corretos

| Campo | Valor |
|---|---|
| **ID** | CT-003-01 |
| **Módulo** | Ordem de Compra |
| **Tipo** | Funcional — Fluxo Principal |
| **Prioridade** | Alta |
| **Pré-condição** | Cotação aprovada pelo gestor / Usuário com perfil COMPRAS autenticado |

**Passos:**
1. Acessar a cotação aprovada
2. Selecionar a opção "Emitir Ordem de Compra"
3. Confirmar os dados preenchidos automaticamente: fornecedor, item, quantidade, valor unitário e total
4. Informar o prazo de entrega acordado
5. Informar a condição de pagamento (ex: 30 dias após recebimento da NF)
6. Clicar em "Emitir PO"

**Resultado esperado:**
- PO gerada com número único sequencial
- Status: "Aguardando confirmação do fornecedor"
- Dados da PO batem exatamente com os da cotação aprovada
- Notificação enviada ao fornecedor com os dados e prazo para confirmação
- Histórico registrado com data, hora e usuário que emitiu

---

### CT-003-02 — Tentativa de emitir PO com valor diferente do cotado aprovado

| Campo | Valor |
|---|---|
| **ID** | CT-003-02 |
| **Módulo** | Ordem de Compra |
| **Tipo** | Validação — Regra de Negócio |
| **Prioridade** | Alta |
| **Pré-condição** | Cotação aprovada disponível |

**Passos:**
1. Acessar a cotação aprovada
2. Selecionar "Emitir Ordem de Compra"
3. Alterar manualmente o valor total para um valor diferente do cotado
4. Tentar emitir a PO

**Resultado esperado:**
- Sistema bloqueia a emissão
- Mensagem: "O valor da ordem de compra não pode divergir do valor aprovado na cotação"
- PO não é gerada

---

### CT-003-03 — Emissão de PO com valor menor que o cotado (cancelamento de saldo)

| Campo | Valor |
|---|---|
| **ID** | CT-003-03 |
| **Módulo** | Ordem de Compra |
| **Tipo** | Funcional — Fluxo Alternativo |
| **Prioridade** | Alta |
| **Pré-condição** | Cotação aprovada disponível |

**Passos:**
1. Acessar a cotação aprovada com valor de R$ 10.000,00
2. Emitir PO com valor de R$ 8.000,00 (menor que o aprovado)
3. Sistema solicita confirmação do cancelamento do saldo restante (R$ 2.000,00)
4. Confirmar o cancelamento do saldo

**Resultado esperado:**
- PO emitida com o valor de R$ 8.000,00
- Saldo de R$ 2.000,00 cancelado e registrado no histórico
- Status da cotação atualizado para refletir o valor final

---

### CT-003-04 — Tentativa de emitir PO sem prazo de entrega

| Campo | Valor |
|---|---|
| **ID** | CT-003-04 |
| **Módulo** | Ordem de Compra |
| **Tipo** | Validação — Campo Obrigatório |
| **Prioridade** | Média |

**Passos:**
1. Acessar cotação aprovada
2. Iniciar emissão da PO
3. Deixar o campo "Prazo de entrega" em branco
4. Tentar emitir

**Resultado esperado:**
- Sistema bloqueia a emissão
- Mensagem: "O prazo de entrega é obrigatório para emissão da ordem de compra"

---

## Casos de Teste — Confirmação pelo Fornecedor

---

### CT-003-05 — Fornecedor confirma recebimento da PO dentro do prazo

| Campo | Valor |
|---|---|
| **ID** | CT-003-05 |
| **Módulo** | Ordem de Compra — Confirmação |
| **Tipo** | Funcional — Fluxo Principal |
| **Prioridade** | Alta |
| **Pré-condição** | PO emitida com status "Aguardando confirmação do fornecedor" |

**Passos:**
1. Fornecedor acessa o sistema ou link enviado por notificação
2. Visualiza os dados da PO: item, quantidade, valor e prazo
3. Clica em "Confirmar recebimento"

**Resultado esperado:**
- Status da PO atualizado para "Confirmada — Em andamento"
- Data e hora da confirmação registradas
- Notificação enviada à equipe de compras informando a confirmação
- Prazo de entrega começa a contar a partir da confirmação

---

### CT-003-06 — Fornecedor não confirma a PO dentro do prazo

| Campo | Valor |
|---|---|
| **ID** | CT-003-06 |
| **Módulo** | Ordem de Compra — Confirmação |
| **Tipo** | Exceção — Prazo |
| **Prioridade** | Alta |
| **Pré-condição** | PO emitida / Prazo de confirmação expirado sem retorno do fornecedor |

**Passos:**
1. Verificar PO com prazo de confirmação vencido
2. Sistema deve identificar automaticamente o vencimento

**Resultado esperado:**
- Status da PO atualizado para "Confirmação pendente — Prazo expirado"
- Alerta enviado automaticamente à equipe de compras
- Sistema registra o evento no histórico da PO
- Equipe pode acionar notificação manual ao fornecedor ou cancelar a PO

---

## Casos de Teste — Cancelamento e Reemissão

---

### CT-003-07 — Cancelamento de PO emitida com valor incorreto e reemissão

| Campo | Valor |
|---|---|
| **ID** | CT-003-07 |
| **Módulo** | Ordem de Compra — Cancelamento |
| **Tipo** | Funcional — Fluxo Alternativo |
| **Prioridade** | Alta |
| **Pré-condição** | PO emitida com valor incorreto / Usuário com perfil COMPRAS autenticado |

**Passos:**
1. Localizar a PO com valor incorreto
2. Selecionar "Cancelar PO"
3. Informar a justificativa: "Valor incorreto — divergência com cotação aprovada"
4. Confirmar o cancelamento
5. Emitir nova PO com o valor correto

**Resultado esperado:**
- PO original cancelada com registro de justificativa e data
- Nova PO emitida vinculada à mesma cotação aprovada
- Histórico mostra o cancelamento e a reemissão em sequência
- Número da nova PO é diferente da cancelada

---

### CT-003-08 — Tentativa de cancelar PO já confirmada pelo fornecedor

| Campo | Valor |
|---|---|
| **ID** | CT-003-08 |
| **Módulo** | Ordem de Compra — Cancelamento |
| **Tipo** | Exceção — Regra de Negócio |
| **Prioridade** | Alta |
| **Pré-condição** | PO com status "Confirmada — Em andamento" |

**Passos:**
1. Localizar PO já confirmada pelo fornecedor
2. Tentar cancelar a PO

**Resultado esperado:**
- Sistema exibe alerta: "Esta PO já foi confirmada pelo fornecedor. O cancelamento requer aprovação do gestor."
- Cancelamento não é executado sem aprovação
- Fluxo de aprovação de cancelamento é iniciado

---

## Casos de Teste — Exceções de Entrega

---

### CT-003-09 — Fornecedor não entrega no prazo acordado

| Campo | Valor |
|---|---|
| **ID** | CT-003-09 |
| **Módulo** | Ordem de Compra — Entrega |
| **Tipo** | Exceção — Prazo de Entrega |
| **Prioridade** | Alta |
| **Pré-condição** | PO confirmada / Data de entrega vencida sem registro de entrega |

**Passos:**
1. Verificar PO com prazo de entrega vencido
2. Registrar ocorrência de atraso no sistema
3. Emitir notificação formal ao fornecedor com novo prazo
4. Registrar evidências da notificação (e-mail, print, protocolo)

**Resultado esperado:**
- Ocorrência de atraso registrada e vinculada à PO
- Status atualizado para "Em atraso"
- Notificação enviada ao fornecedor com registro de data e hora
- Histórico completo disponível para auditoria e eventuais penalidades contratuais

---

## Resumo dos Casos de Teste

| ID | Descrição | Tipo | Prioridade |
|---|---|---|---|
| CT-003-01 | Emissão de PO válida | Fluxo principal | Alta |
| CT-003-02 | PO com valor divergente do cotado | Validação | Alta |
| CT-003-03 | PO com valor menor — cancelamento de saldo | Fluxo alternativo | Alta |
| CT-003-04 | PO sem prazo de entrega | Campo obrigatório | Média |
| CT-003-05 | Fornecedor confirma PO no prazo | Fluxo principal | Alta |
| CT-003-06 | Fornecedor não confirma no prazo | Exceção | Alta |
| CT-003-07 | Cancelamento e reemissão de PO | Fluxo alternativo | Alta |
| CT-003-08 | Cancelamento de PO já confirmada | Exceção | Alta |
| CT-003-09 | Fornecedor não entrega no prazo | Exceção | Alta |
