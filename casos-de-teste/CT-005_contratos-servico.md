# CT-005 — Contratos de Serviço

## Contexto

A contratação de serviços segue o mesmo fluxo de requisição, cotação e aprovação da aquisição de materiais. O que diferencia é a **execução contratual** — o acompanhamento do serviço prestado após a contratação.

Existem dois tipos principais de serviço:

- **Serviço pontual:** empresa executa uma atividade específica (ex: dedetização, manutenção, consultoria). Execução única ou eventual. O pagamento é realizado após ateste de que o serviço foi executado corretamente.
- **Mão de obra (posto de serviço):** empresa fornece equipe para atuação contínua (ex: limpeza, segurança, recepção). O contrato é por **posto de serviço**, não por pessoa. A empresa é responsável por repor ausências. O pagamento mensal é baseado no ateste dos postos efetivamente ocupados.

**Perfis de usuário:**
- `SOLICITANTE` — Gestor que solicita e atesta o serviço
- `COMPRAS` — Equipe que formaliza o contrato
- `APROVADOR` — Gestor financeiro que autoriza pagamento
- `FINANCEIRO` — Processa o pagamento após ateste

---

## Casos de Teste — Serviço Pontual

---

### CT-005-01 — Registro de execução de serviço pontual concluído corretamente

| Campo | Valor |
|---|---|
| **ID** | CT-005-01 |
| **Módulo** | Contratos de Serviço — Serviço Pontual |
| **Tipo** | Funcional — Fluxo Principal |
| **Prioridade** | Alta |
| **Pré-condição** | Contrato de serviço ativo / Serviço executado pelo fornecedor / Usuário com perfil SOLICITANTE autenticado |

**Passos:**
1. Acessar o módulo de execução contratual
2. Localizar o contrato de serviço vinculado
3. Registrar a execução do serviço com data e descrição
4. Emitir o **ateste de execução** confirmando que o serviço foi realizado corretamente
5. Encaminhar para o financeiro para pagamento da NF

**Resultado esperado:**
- Ateste registrado com data, hora e nome do responsável
- Status do contrato atualizado para "Serviço atestado — Aguardando NF"
- Notificação enviada ao financeiro
- Histórico de execução disponível para auditoria

---

### CT-005-02 — Registro de serviço pontual executado de forma inadequada

| Campo | Valor |
|---|---|
| **ID** | CT-005-02 |
| **Módulo** | Contratos de Serviço — Serviço Pontual |
| **Tipo** | Funcional — Fluxo Alternativo |
| **Prioridade** | Alta |
| **Pré-condição** | Contrato de serviço ativo / Serviço executado com qualidade insatisfatória |

**Passos:**
1. Acessar o módulo de execução contratual
2. Localizar o contrato vinculado
3. Selecionar "Registrar ocorrência"
4. Descrever a não conformidade (ex: "Serviço de dedetização não cobriu todas as áreas contratadas")
5. Anexar evidências (fotos, relatos, e-mails de reclamação)
6. Selecionar "Recusar ateste — Solicitar reexecução"

**Resultado esperado:**
- Ocorrência registrada e vinculada ao contrato
- Status atualizado para "Serviço recusado — Reexecução solicitada"
- Notificação enviada ao fornecedor com prazo para reexecução
- NF bloqueada para pagamento até resolução
- Histórico completo da ocorrência disponível

---

### CT-005-03 — Fornecedor envia NF antes do ateste de execução

| Campo | Valor |
|---|---|
| **ID** | CT-005-03 |
| **Módulo** | Contratos de Serviço — Serviço Pontual |
| **Tipo** | Validação — Regra de Negócio |
| **Prioridade** | Alta |

**Passos:**
1. Financeiro tenta liquidar NF de serviço
2. Verificar que o ateste de execução ainda não foi emitido pelo solicitante

**Resultado esperado:**
- Sistema bloqueia a liquidação da NF
- Mensagem: "Esta NF não pode ser liquidada. O ateste de execução do serviço ainda não foi registrado."
- NF permanece pendente até o ateste ser emitido

---

## Casos de Teste — Mão de Obra (Posto de Serviço)

---

### CT-005-04 — Ateste mensal com todos os postos ocupados

| Campo | Valor |
|---|---|
| **ID** | CT-005-04 |
| **Módulo** | Contratos de Serviço — Mão de Obra |
| **Tipo** | Funcional — Fluxo Principal |
| **Prioridade** | Alta |
| **Pré-condição** | Contrato de mão de obra ativo / Mês encerrado / Todos os postos ocupados integralmente |

**Passos:**
1. Acessar o módulo de ateste mensal
2. Localizar o contrato de mão de obra
3. Confirmar que todos os postos contratados foram ocupados durante o mês
4. Emitir ateste mensal integral
5. Encaminhar para pagamento da NF mensal

**Resultado esperado:**
- Ateste mensal registrado com o período e quantidade de postos atestados
- NF mensal encaminhada ao financeiro pelo valor integral do contrato
- Histórico do ateste disponível para auditoria

---

### CT-005-05 — Registro de falta de funcionário sem reposição pelo fornecedor

| Campo | Valor |
|---|---|
| **ID** | CT-005-05 |
| **Módulo** | Contratos de Serviço — Mão de Obra |
| **Tipo** | Funcional — Fluxo Alternativo |
| **Prioridade** | Alta |
| **Pré-condição** | Contrato de posto de serviço ativo / Funcionário ausente sem reposição |

**Passos:**
1. Acessar o módulo de controle de postos
2. Registrar a ausência do funcionário com data
3. Registrar que o fornecedor não realizou a reposição
4. Vincular a ocorrência ao contrato

**Resultado esperado:**
- Ausência registrada com data e identificação do posto afetado
- Ocorrência de não reposição registrada e vinculada ao contrato
- Sistema calcula automaticamente o desconto proporcional na NF mensal
- Notificação enviada ao fornecedor informando o desconto a ser aplicado

---

### CT-005-06 — Tentativa de atestar posto não ocupado como ocupado

| Campo | Valor |
|---|---|
| **ID** | CT-005-06 |
| **Módulo** | Contratos de Serviço — Mão de Obra |
| **Tipo** | Validação — Integridade de Dados |
| **Prioridade** | Alta |

**Passos:**
1. Acessar o módulo de ateste mensal
2. Tentar atestar integralmente um contrato que possui ocorrências de ausência registradas no mês

**Resultado esperado:**
- Sistema alerta sobre as ocorrências de ausência registradas no período
- Mensagem: "Existem X ocorrências de ausência registradas neste período. O ateste integral não é possível. Revise antes de confirmar."
- Ateste integral bloqueado até que as ocorrências sejam tratadas

---

### CT-005-07 — Fornecedor quer cobrar por posto não ocupado

| Campo | Valor |
|---|---|
| **ID** | CT-005-07 |
| **Módulo** | Contratos de Serviço — Mão de Obra |
| **Tipo** | Exceção — Divergência de Cobrança |
| **Prioridade** | Alta |
| **Pré-condição** | NF enviada pelo fornecedor com valor superior ao atestado |

**Passos:**
1. Financeiro recebe NF com valor integral
2. Sistema compara o valor da NF com o valor calculado a partir do ateste (com descontos de ausência)
3. Sistema identifica divergência de valor

**Resultado esperado:**
- Sistema alerta sobre a divergência entre o valor da NF e o valor atestado
- Mensagem: "O valor da NF diverge do valor atestado. NF atestada: R$ X. NF recebida: R$ Y."
- NF contestada automaticamente e encaminhada ao fornecedor para correção
- Registro da contestação disponível para auditoria e histórico contratual

---

## Casos de Teste — Penalidades e Encerramento

---

### CT-005-08 — Registro de notificação formal por descumprimento contratual

| Campo | Valor |
|---|---|
| **ID** | CT-005-08 |
| **Módulo** | Contratos de Serviço — Penalidades |
| **Tipo** | Funcional — Fluxo de Exceção |
| **Prioridade** | Alta |
| **Pré-condição** | Ocorrências de descumprimento registradas no contrato |

**Passos:**
1. Acessar o contrato com ocorrências registradas
2. Selecionar "Emitir notificação formal"
3. Descrever o descumprimento e o prazo para regularização
4. Registrar evidências (e-mails, prints, relatos)
5. Confirmar o envio da notificação

**Resultado esperado:**
- Notificação formal registrada e vinculada ao contrato
- Data de envio e prazo de resposta registrados
- Status do contrato atualizado para "Em notificação"
- Histórico completo disponível para eventual aplicação de penalidade ou rescisão

---

### CT-005-09 — Encerramento de contrato de serviço ao fim da vigência

| Campo | Valor |
|---|---|
| **ID** | CT-005-09 |
| **Módulo** | Contratos de Serviço — Encerramento |
| **Tipo** | Funcional — Fluxo Principal |
| **Prioridade** | Média |
| **Pré-condição** | Contrato com data de vigência encerrada |

**Passos:**
1. Sistema identifica contratos com vigência vencida
2. Status do contrato atualizado automaticamente para "Encerrado"
3. Gestor responsável recebe notificação de encerramento

**Resultado esperado:**
- Contrato encerrado automaticamente na data de vencimento
- Nenhuma nova NF pode ser vinculada ao contrato encerrado
- Notificação enviada ao gestor e à equipe de compras
- Histórico completo do contrato disponível para consulta

---

### CT-005-10 — Tentativa de atestar serviço em contrato encerrado

| Campo | Valor |
|---|---|
| **ID** | CT-005-10 |
| **Módulo** | Contratos de Serviço — Encerramento |
| **Tipo** | Exceção — Regra de Negócio |
| **Prioridade** | Alta |

**Passos:**
1. Tentar registrar ateste de serviço em contrato com status "Encerrado"

**Resultado esperado:**
- Sistema bloqueia o ateste
- Mensagem: "Este contrato está encerrado e não aceita novos registros de execução."
- Nenhum ateste é gerado

---

## Resumo dos Casos de Teste

| ID | Descrição | Tipo | Prioridade |
|---|---|---|---|
| CT-005-01 | Serviço pontual executado corretamente | Fluxo principal | Alta |
| CT-005-02 | Serviço pontual executado inadequadamente | Fluxo alternativo | Alta |
| CT-005-03 | NF antes do ateste de execução | Validação | Alta |
| CT-005-04 | Ateste mensal com todos os postos ocupados | Fluxo principal | Alta |
| CT-005-05 | Falta sem reposição pelo fornecedor | Fluxo alternativo | Alta |
| CT-005-06 | Ateste integral com ocorrências registradas | Validação | Alta |
| CT-005-07 | Fornecedor cobra por posto não ocupado | Exceção | Alta |
| CT-005-08 | Notificação formal por descumprimento | Exceção | Alta |
| CT-005-09 | Encerramento de contrato ao fim da vigência | Fluxo principal | Média |
| CT-005-10 | Ateste em contrato encerrado | Exceção | Alta |
