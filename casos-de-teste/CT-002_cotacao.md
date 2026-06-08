# CT-002 — Cotação de Preços

## Contexto

Após recebimento da requisição, a equipe de compras analisa o valor estimado e define o caminho da aquisição conforme a política interna de compras da organização:

- **Compra direta:** valor dentro do limite da política interna → mínimo de 3 cotações de fornecedores distintos
- **Processo competitivo (RFQ/concorrência):** valor acima do limite → pesquisa de preços para composição do processo seletivo
- **Acordo de Fornecimento ou Contrato vigente:** item já coberto → cotação dispensada, avança direto para ordem de compra

Quando não é possível obter o número mínimo de cotações, a equipe deve justificar com evidências (prints de tela, e-mails enviados, pesquisas de mercado realizadas).

**Perfis de usuário:**
- `COMPRAS` — Equipe de compras (executa a cotação)
- `APROVADOR` — Gestor financeiro (aprova a escolha do fornecedor)

---

## Casos de Teste — Compra Direta

---

### CT-002-01 — Cotação válida com 3 fornecedores

| Campo | Valor |
|---|---|
| **ID** | CT-002-01 |
| **Módulo** | Cotação de Preços — Compra Direta |
| **Tipo** | Funcional — Fluxo Principal |
| **Prioridade** | Alta |
| **Pré-condição** | Requisição recebida pela equipe de compras / Usuário com perfil COMPRAS autenticado |

**Passos:**
1. Acessar a requisição recebida
2. Selecionar a opção "Compra Direta"
3. Cadastrar cotação do Fornecedor 1: nome, CNPJ, valor unitário, valor total
4. Cadastrar cotação do Fornecedor 2: nome, CNPJ, valor unitário, valor total
5. Cadastrar cotação do Fornecedor 3: nome, CNPJ, valor unitário, valor total
6. Selecionar o fornecedor com menor preço
7. Clicar em "Encaminhar para aprovação"

**Resultado esperado:**
- Sistema registra as 3 cotações vinculadas à requisição
- Fornecedor com menor valor destacado automaticamente
- Status alterado para "Aguardando aprovação do gestor"
- Histórico de cotações disponível para consulta e auditoria

---

### CT-002-02 — Tentativa de finalizar cotação com menos de 3 fornecedores sem justificativa

| Campo | Valor |
|---|---|
| **ID** | CT-002-02 |
| **Módulo** | Cotação de Preços — Compra Direta |
| **Tipo** | Validação — Regra de Negócio |
| **Prioridade** | Alta |
| **Pré-condição** | Solicitação aprovada / Usuário com perfil COMPRAS autenticado |

**Passos:**
1. Acessar a solicitação aprovada
2. Selecionar "Compra Direta"
3. Cadastrar cotação de apenas 1 fornecedor
4. Tentar clicar em "Encaminhar para aprovação" sem justificativa

**Resultado esperado:**
- Sistema bloqueia o avanço
- Mensagem: "São necessárias ao menos 3 cotações ou justificativa para número inferior"
- Nenhum encaminhamento realizado

---

### CT-002-03 — Cotação com menos de 3 fornecedores com justificativa e evidências

| Campo | Valor |
|---|---|
| **ID** | CT-002-03 |
| **Módulo** | Cotação de Preços — Compra Direta |
| **Tipo** | Funcional — Fluxo Alternativo |
| **Prioridade** | Alta |
| **Pré-condição** | Solicitação aprovada / Usuário com perfil COMPRAS autenticado |

**Passos:**
1. Acessar a solicitação aprovada
2. Selecionar "Compra Direta"
3. Cadastrar cotação de apenas 2 fornecedores
4. Preencher o campo "Justificativa" com "Produto com fornecedores restritos no mercado regional"
5. Anexar evidências (print de e-mails enviados, pesquisa de mercado)
6. Clicar em "Encaminhar para aprovação"

**Resultado esperado:**
- Sistema aceita o encaminhamento com a justificativa registrada
- Evidências anexadas e vinculadas à solicitação
- Status alterado para "Aguardando aprovação do gestor"
- Justificativa visível no histórico para fins de auditoria

---

### CT-002-04 — Cotação com CNPJ inválido

| Campo | Valor |
|---|---|
| **ID** | CT-002-04 |
| **Módulo** | Cotação de Preços |
| **Tipo** | Validação — Dado Inválido |
| **Prioridade** | Média |
| **Pré-condição** | Usuário com perfil COMPRAS autenticado |

**Passos:**
1. Cadastrar cotação de fornecedor
2. Informar CNPJ com formato inválido (ex: "12.345.678/0001-00" com dígito verificador incorreto)
3. Tentar salvar

**Resultado esperado:**
- Sistema valida o CNPJ e exibe mensagem de erro
- Mensagem: "CNPJ inválido. Verifique o número informado."
- Cotação não é salva

---

### CT-002-05 — Cotação com valor zero

| Campo | Valor |
|---|---|
| **ID** | CT-002-05 |
| **Módulo** | Cotação de Preços |
| **Tipo** | Validação — Valor de Limite |
| **Prioridade** | Média |

**Passos:**
1. Cadastrar cotação de fornecedor
2. Informar valor unitário = **R$ 0,00**
3. Tentar salvar

**Resultado esperado:**
- Sistema bloqueia o salvamento
- Mensagem: "O valor da cotação deve ser maior que zero"

---

## Casos de Teste — Acordo de Fornecimento Vigente

---

### CT-002-06 — Item coberto por acordo de fornecimento vigente — direto para ordem de compra

| Campo | Valor |
|---|---|
| **ID** | CT-002-06 |
| **Módulo** | Cotação de Preços — Acordo de Fornecimento |
| **Tipo** | Funcional — Fluxo Alternativo |
| **Prioridade** | Alta |
| **Pré-condição** | Requisição recebida / Acordo de fornecimento vigente cadastrado no sistema para o item solicitado |

**Passos:**
1. Acessar a requisição recebida
2. Verificar se existe acordo de fornecimento vigente para o item
3. Selecionar a opção "Utilizar Acordo de Fornecimento"
4. Sistema exibe os dados do acordo: fornecedor, valor registrado, saldo disponível, validade
5. Confirmar utilização do acordo

**Resultado esperado:**
- Cotação dispensada automaticamente
- Requisição avança diretamente para emissão de Ordem de Compra
- Dados do acordo vinculados à requisição (número, fornecedor, valor, validade)
- Saldo do acordo atualizado após o vínculo

---

### CT-002-07 — Tentativa de utilizar acordo de fornecimento com validade expirada

| Campo | Valor |
|---|---|
| **ID** | CT-002-07 |
| **Módulo** | Cotação de Preços — Acordo de Fornecimento |
| **Tipo** | Exceção — Regra de Negócio |
| **Prioridade** | Alta |

**Passos:**
1. Acessar requisição recebida
2. Tentar selecionar um acordo de fornecimento com data de validade expirada

**Resultado esperado:**
- Sistema bloqueia a seleção
- Mensagem: "Acordo de fornecimento expirado. Validade: [data]. Realize nova cotação."
- Acordo expirado não pode ser utilizado

---

### CT-002-08 — Tentativa de utilizar acordo de fornecimento com saldo insuficiente

| Campo | Valor |
|---|---|
| **ID** | CT-002-08 |
| **Módulo** | Cotação de Preços — Acordo de Fornecimento |
| **Tipo** | Exceção — Regra de Negócio |
| **Prioridade** | Alta |

**Passos:**
1. Acessar requisição com quantidade solicitada superior ao saldo do acordo
2. Tentar selecionar o acordo de fornecimento

**Resultado esperado:**
- Sistema alerta sobre saldo insuficiente
- Mensagem: "Saldo insuficiente no acordo. Saldo disponível: [X unidades]. Quantidade solicitada: [Y unidades]."
- Usuário pode ajustar a quantidade ou optar por nova cotação para o excedente

---

## Casos de Teste — Contrato Vigente

---

### CT-002-09 — Item coberto por contrato vigente — ordem de serviço

| Campo | Valor |
|---|---|
| **ID** | CT-002-09 |
| **Módulo** | Cotação de Preços — Contrato |
| **Tipo** | Funcional — Fluxo Alternativo |
| **Prioridade** | Alta |
| **Pré-condição** | Contrato vigente cadastrado no sistema para o serviço/item solicitado |

**Passos:**
1. Acessar a solicitação aprovada
2. Verificar existência de contrato vigente
3. Selecionar "Utilizar Contrato Vigente"
4. Sistema exibe os dados do contrato: fornecedor, objeto, valor, vigência
5. Emitir Ordem de Serviço vinculada ao contrato

**Resultado esperado:**
- Cotação dispensada
- Ordem de Serviço gerada e vinculada ao contrato
- Solicitação avança para execução/entrega
- Histórico registrado com número do contrato utilizado

---

## Resumo dos Casos de Teste

| ID | Descrição | Tipo | Prioridade |
|---|---|---|---|
| CT-002-01 | Cotação válida com 3 fornecedores | Fluxo principal | Alta |
| CT-002-02 | Menos de 3 fornecedores sem justificativa | Validação | Alta |
| CT-002-03 | Menos de 3 fornecedores com justificativa | Fluxo alternativo | Alta |
| CT-002-04 | CNPJ inválido | Dado inválido | Média |
| CT-002-05 | Valor de cotação zero | Limite de valor | Média |
| CT-002-06 | Acordo de fornecimento vigente — direto para ordem de compra | Fluxo alternativo | Alta |
| CT-002-07 | Acordo de fornecimento expirado | Exceção | Alta |
| CT-002-08 | Acordo de fornecimento com saldo insuficiente | Exceção | Alta |
| CT-002-09 | Contrato vigente — ordem de serviço | Fluxo alternativo | Alta |
