# CT-001 — Solicitação de Compra

## Contexto

A solicitação de compra é a etapa inicial do ciclo Procure-to-Pay. O solicitante (diretor de unidade ou chefe de divisão) preenche um **Termo de Referência** descrevendo o que deseja adquirir. O documento segue para aprovação do ordenador de despesa antes de avançar para cotação.

**Base legal:** Lei 14.133/2021 — Nova Lei de Licitações e Contratos Administrativos.

**Perfis de usuário:**
- `SOLICITANTE` — Diretor de unidade / Chefe de divisão (pode abrir solicitação)
- `ORDENADOR` — Secretário / Ordenador de despesa (aprova ou reprova)
- `COMPRAS` — Equipe de compras (recebe após aprovação)

---

## Casos de Teste — Fluxo Principal

---

### CT-001-01 — Solicitação válida com todos os campos obrigatórios

| Campo | Valor |
|---|---|
| **ID** | CT-001-01 |
| **Módulo** | Solicitação de Compra |
| **Tipo** | Funcional — Fluxo Principal |
| **Prioridade** | Alta |
| **Pré-condição** | Usuário autenticado com perfil SOLICITANTE |

**Passos:**
1. Acessar o módulo de Solicitação de Compra
2. Clicar em "Nova Solicitação"
3. Preencher o campo **O que deseja adquirir** com "Material escolar — cadernos, lápis e borrachas"
4. Preencher o campo **Quantidade** com "500 unidades"
5. Preencher o campo **Justificativa / Finalidade** com "Atendimento às turmas do ensino fundamental para o ano letivo"
6. Selecionar o **Tipo de aquisição** como "Aquisição de materiais"
7. Clicar em "Enviar para aprovação"

**Resultado esperado:**
- Solicitação criada com número de protocolo gerado automaticamente
- Status alterado para "Aguardando aprovação do ordenador"
- Notificação enviada ao ordenador de despesa
- Solicitante visualiza o registro na lista de solicitações com status atualizado

---

### CT-001-02 — Tentativa de envio com campos obrigatórios em branco

| Campo | Valor |
|---|---|
| **ID** | CT-001-02 |
| **Módulo** | Solicitação de Compra |
| **Tipo** | Funcional — Validação de Campos |
| **Prioridade** | Alta |
| **Pré-condição** | Usuário autenticado com perfil SOLICITANTE |

**Passos:**
1. Acessar o módulo de Solicitação de Compra
2. Clicar em "Nova Solicitação"
3. Deixar todos os campos obrigatórios em branco
4. Clicar em "Enviar para aprovação"

**Resultado esperado:**
- Sistema bloqueia o envio
- Mensagem de erro exibida indicando quais campos são obrigatórios
- Nenhuma solicitação é criada no banco de dados
- Usuário permanece na tela de preenchimento

---

### CT-001-03 — Tentativa de envio sem justificativa/finalidade

| Campo | Valor |
|---|---|
| **ID** | CT-001-03 |
| **Módulo** | Solicitação de Compra |
| **Tipo** | Funcional — Validação de Campo |
| **Prioridade** | Alta |
| **Pré-condição** | Usuário autenticado com perfil SOLICITANTE |

**Passos:**
1. Acessar o módulo de Solicitação de Compra
2. Clicar em "Nova Solicitação"
3. Preencher "O que deseja adquirir" e "Quantidade"
4. Deixar o campo "Justificativa / Finalidade" em branco
5. Clicar em "Enviar para aprovação"

**Resultado esperado:**
- Sistema bloqueia o envio
- Mensagem de erro específica para o campo "Justificativa"
- Nenhuma solicitação é criada

> **Nota:** A justificativa é obrigatória pela Lei 14.133/2021 — toda contratação pública deve ter finalidade comprovada.

---

### CT-001-04 — Usuário sem perfil autorizado tenta abrir solicitação

| Campo | Valor |
|---|---|
| **ID** | CT-001-04 |
| **Módulo** | Solicitação de Compra |
| **Tipo** | Controle de Acesso |
| **Prioridade** | Alta |
| **Pré-condição** | Usuário autenticado com perfil sem permissão de solicitação (ex: perfil operacional) |

**Passos:**
1. Acessar o módulo de Solicitação de Compra com usuário não autorizado
2. Tentar acessar "Nova Solicitação"

**Resultado esperado:**
- Botão "Nova Solicitação" não está visível, OU
- Sistema exibe mensagem "Você não tem permissão para realizar esta ação"
- Nenhuma solicitação é criada

---

## Casos de Teste — Fluxo de Aprovação

---

### CT-001-05 — Ordenador aprova solicitação

| Campo | Valor |
|---|---|
| **ID** | CT-001-05 |
| **Módulo** | Solicitação de Compra — Aprovação |
| **Tipo** | Funcional — Fluxo de Aprovação |
| **Prioridade** | Alta |
| **Pré-condição** | Solicitação criada com status "Aguardando aprovação" / Usuário com perfil ORDENADOR autenticado |

**Passos:**
1. Acessar a lista de solicitações pendentes de aprovação
2. Localizar a solicitação criada no CT-001-01
3. Visualizar o Termo de Referência completo
4. Clicar em "Aprovar"

**Resultado esperado:**
- Status da solicitação alterado para "Aprovada — Aguardando cotação"
- Solicitação encaminhada para a equipe de compras
- Notificação enviada ao solicitante informando a aprovação
- Registro de log com data, hora e nome do ordenador que aprovou

---

### CT-001-06 — Ordenador reprova solicitação e devolve para ajuste

| Campo | Valor |
|---|---|
| **ID** | CT-001-06 |
| **Módulo** | Solicitação de Compra — Reprovação |
| **Tipo** | Funcional — Fluxo Alternativo |
| **Prioridade** | Alta |
| **Pré-condição** | Solicitação criada com status "Aguardando aprovação" / Usuário com perfil ORDENADOR autenticado |

**Passos:**
1. Acessar a lista de solicitações pendentes
2. Localizar a solicitação
3. Clicar em "Reprovar / Devolver"
4. Preencher o campo obrigatório de justificativa da reprovação
5. Confirmar a devolução

**Resultado esperado:**
- Status da solicitação alterado para "Devolvida para ajuste"
- Notificação enviada ao solicitante com o motivo da devolução
- Solicitante consegue editar e reenviar a solicitação
- Histórico de tramitação registrado com a justificativa

---

## Casos de Teste — Exceções

---

### CT-001-07 — Solicitação com quantidade igual a zero

| Campo | Valor |
|---|---|
| **ID** | CT-001-07 |
| **Módulo** | Solicitação de Compra |
| **Tipo** | Validação — Valor de Limite |
| **Prioridade** | Média |

**Passos:**
1. Preencher todos os campos obrigatórios
2. Informar quantidade = **0**
3. Clicar em "Enviar"

**Resultado esperado:**
- Sistema bloqueia o envio
- Mensagem: "A quantidade deve ser maior que zero"

---

### CT-001-08 — Solicitação com quantidade negativa

| Campo | Valor |
|---|---|
| **ID** | CT-001-08 |
| **Módulo** | Solicitação de Compra |
| **Tipo** | Validação — Valor Inválido |
| **Prioridade** | Média |

**Passos:**
1. Preencher todos os campos obrigatórios
2. Informar quantidade = **-10**
3. Clicar em "Enviar"

**Resultado esperado:**
- Sistema bloqueia o envio ou não aceita valores negativos no campo
- Mensagem de erro adequada exibida

---

### CT-001-09 — Sessão expirada durante preenchimento

| Campo | Valor |
|---|---|
| **ID** | CT-001-09 |
| **Módulo** | Solicitação de Compra |
| **Tipo** | Exceção — Sessão |
| **Prioridade** | Baixa |

**Passos:**
1. Iniciar o preenchimento de uma nova solicitação
2. Aguardar o tempo de expiração da sessão sem interagir com o sistema
3. Tentar enviar o formulário após a expiração

**Resultado esperado:**
- Sistema redireciona para a tela de login com mensagem "Sessão expirada"
- Dados preenchidos são preservados (comportamento desejável) OU
- Sistema avisa antes da expiração para o usuário salvar o rascunho

---

## Resumo dos Casos de Teste

| ID | Descrição | Tipo | Prioridade |
|---|---|---|---|
| CT-001-01 | Solicitação válida completa | Fluxo principal | Alta |
| CT-001-02 | Envio com todos os campos em branco | Validação | Alta |
| CT-001-03 | Envio sem justificativa | Validação | Alta |
| CT-001-04 | Usuário sem permissão | Controle de acesso | Alta |
| CT-001-05 | Ordenador aprova solicitação | Fluxo de aprovação | Alta |
| CT-001-06 | Ordenador reprova e devolve | Fluxo alternativo | Alta |
| CT-001-07 | Quantidade zero | Limite de valor | Média |
| CT-001-08 | Quantidade negativa | Valor inválido | Média |
| CT-001-09 | Sessão expirada | Exceção | Baixa |
