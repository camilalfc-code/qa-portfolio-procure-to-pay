# Bug Reports — Sistema de Gestão de Compras e Contratos

> Bug reports identificados durante testes exploratórios e funcionais no sistema de Gestão de Compras e Contratos (ciclo Procure-to-Pay).

---

## BUG-001 — Campo de valor aceita vírgula e ponto sem validação consistente

| Campo | Valor |
|---|---|
| **ID** | BUG-001 |
| **Módulo** | Cotação de Preços / Ordem de Compra |
| **Tipo** | Validação de Campo |
| **Severidade** | Alta |
| **Prioridade** | Alta |
| **Status** | Aberto |
| **Reportado em** | 2026-06-10 |

**Descrição:**
O sistema aceita tanto ponto quanto vírgula como separador decimal no campo de valor, sem validação consistente. Dependendo do separador utilizado, o valor é interpretado de forma incorreta, gerando divergências entre o valor digitado e o valor armazenado no banco de dados.

**Passos para reproduzir:**
1. Acessar o módulo de Cotação de Preços
2. Preencher o campo "Valor unitário" com `1.500,00`
3. Salvar o registro
4. Verificar o valor armazenado no banco de dados

**Resultado obtido:**
- O sistema interpreta `1.500,00` como `1,5` (ignorando os zeros após a vírgula) ou armazena valor incorreto dependendo do contexto

**Resultado esperado:**
- O sistema deve aceitar apenas o padrão definido (vírgula como separador decimal no padrão BR) e exibir mensagem de erro clara caso o formato seja inválido

**Evidências:**
- Print do campo com valor digitado vs valor salvo
- Consulta SQL mostrando o valor incorreto no banco

---

## BUG-002 — Sistema apresenta lentidão crítica durante consultas com múltiplos filtros

| Campo | Valor |
|---|---|
| **ID** | BUG-002 |
| **Módulo** | Consulta / Pesquisa Geral |
| **Tipo** | Performance |
| **Severidade** | Alta |
| **Prioridade** | Alta |
| **Status** | Aberto |
| **Reportado em** | 2026-06-10 |

**Descrição:**
O sistema apresenta lentidão crítica ao realizar consultas com múltiplos filtros simultâneos (ex: nome do fornecedor + período + status). O tempo de resposta ultrapassa 30 segundos, prejudicando a operação diária dos usuários.

**Passos para reproduzir:**
1. Acessar o módulo de consulta de contratos
2. Aplicar os filtros: Nome do fornecedor + Período (data início e fim) + Status "Ativo"
3. Clicar em "Pesquisar"
4. Cronometrar o tempo de resposta

**Resultado obtido:**
- Tempo de resposta superior a 30 segundos
- Em alguns casos a página exibe erro de timeout ou fica em carregamento indefinido

**Resultado esperado:**
- Consultas com filtros combinados devem retornar resultados em até 3 segundos

**Evidências:**
- Print da tela com indicador de carregamento
- Registro do tempo de resposta

---

## BUG-003 — Falha ao anexar documentos acima de determinado tamanho

| Campo | Valor |
|---|---|
| **ID** | BUG-003 |
| **Módulo** | Gestão de Contratos / Anexos |
| **Tipo** | Funcional |
| **Severidade** | Alta |
| **Prioridade** | Alta |
| **Status** | Aberto |
| **Reportado em** | 2026-06-10 |

**Descrição:**
O sistema falha ao tentar anexar documentos sem exibir mensagem de erro clara ao usuário. O arquivo parece ser carregado (barra de progresso avança) mas não é salvo, e o usuário só percebe a falha ao tentar consultar o anexo posteriormente.

**Passos para reproduzir:**
1. Acessar um contrato ativo
2. Clicar em "Anexar documento"
3. Selecionar um arquivo PDF
4. Aguardar o carregamento
5. Salvar e reabrir o registro para verificar o anexo

**Resultado obtido:**
- A barra de progresso indica 100% de carregamento
- Ao reabrir o registro, o anexo não está disponível
- Nenhuma mensagem de erro é exibida durante o processo

**Resultado esperado:**
- O sistema deve salvar o anexo corretamente e confirmar com mensagem de sucesso
- Em caso de falha, deve exibir mensagem de erro clara informando o motivo (ex: tamanho máximo excedido, formato não suportado)

**Evidências:**
- Print da tela após o carregamento sem o anexo salvo
- Print do registro sem o documento anexado

---

## BUG-004 — Filtro de busca por nome do fornecedor não retorna resultados parciais

| Campo | Valor |
|---|---|
| **ID** | BUG-004 |
| **Módulo** | Consulta de Fornecedores / Contratos |
| **Tipo** | Funcional |
| **Severidade** | Média |
| **Prioridade** | Alta |
| **Status** | Aberto |
| **Reportado em** | 2026-06-10 |

**Descrição:**
O filtro de busca por nome do fornecedor só retorna resultados quando o nome é digitado de forma exata e completa. Buscas parciais ou com variações de maiúsculas/minúsculas não retornam resultados, dificultando a localização de registros.

**Passos para reproduzir:**
1. Acessar o módulo de consulta de contratos
2. No campo "Nome do fornecedor", digitar parte do nome: ex: `Limpeza`
3. Clicar em "Pesquisar"

**Resultado obtido:**
- Nenhum resultado retornado, mesmo existindo fornecedores com "Limpeza" no nome

**Resultado esperado:**
- O sistema deve retornar todos os fornecedores que contenham o termo pesquisado em qualquer parte do nome (busca parcial com LIKE)
- A busca deve ser case-insensitive (indiferente a maiúsculas e minúsculas)

**Evidências:**
- Print da busca sem resultados
- Print do cadastro do fornecedor confirmando que o registro existe

---

## BUG-005 — Falha na integração com portal externo — publicação não é processada

| Campo | Valor |
|---|---|
| **ID** | BUG-005 |
| **Módulo** | Integração com Portal Externo |
| **Tipo** | Integração |
| **Severidade** | Crítica |
| **Prioridade** | Alta |
| **Status** | Aberto |
| **Reportado em** | 2026-06-10 |

**Descrição:**
Ao registrar um aditivo contratual e acionar o envio para publicação no portal externo de contratos, o sistema confirma o envio mas a publicação não é processada no portal. O usuário só identifica a falha ao consultar o portal diretamente, sem nenhum alerta no sistema.

**Passos para reproduzir:**
1. Acessar um contrato ativo
2. Registrar um aditivo (ex: aditivo de prazo)
3. Anexar o documento do aditivo
4. Acionar "Publicar no portal externo"
5. Sistema exibe mensagem de "Enviado com sucesso"
6. Acessar o portal externo e verificar se a publicação foi processada

**Resultado obtido:**
- Sistema exibe mensagem de sucesso no envio
- Portal externo não registra a publicação
- Nenhum alerta de falha é exibido ao usuário

**Resultado esperado:**
- O sistema deve confirmar o processamento no portal externo, não apenas o envio
- Em caso de falha na integração, deve exibir mensagem de erro e registrar log da falha
- O usuário deve ser notificado para reenviar ou acionar suporte

**Evidências:**
- Print da mensagem de sucesso no sistema
- Print do portal externo sem a publicação registrada
- Log de integração (se disponível)

---

## BUG-006 — Valor aditado não reflete no saldo do contrato após confirmação

| Campo | Valor |
|---|---|
| **ID** | BUG-006 |
| **Módulo** | Gestão de Contratos — Aditivos |
| **Tipo** | Funcional |
| **Severidade** | Crítica |
| **Prioridade** | Alta |
| **Status** | Aberto |
| **Reportado em** | 2026-06-10 |

**Descrição:**
Após registrar um aditivo de valor em um contrato e confirmar a operação, o novo valor não é atualizado no saldo do contrato. O sistema mantém o valor original, gerando inconsistência entre o valor aditado e o saldo disponível para empenho.

**Passos para reproduzir:**
1. Acessar um contrato ativo com valor de R$ 50.000,00
2. Registrar aditivo de valor no montante de R$ 10.000,00
3. Confirmar o aditivo
4. Verificar o saldo disponível do contrato

**Resultado obtido:**
- Saldo do contrato permanece em R$ 50.000,00
- O aditivo aparece registrado no histórico mas não impacta o saldo

**Resultado esperado:**
- Após confirmação do aditivo, o saldo do contrato deve ser atualizado para R$ 60.000,00
- O histórico deve registrar: valor original + valor aditado = novo valor total

**Evidências:**
- Print do contrato antes do aditivo
- Print do aditivo registrado
- Print do saldo do contrato sem atualização
- Consulta SQL mostrando o valor no banco de dados

---

## Resumo dos Bug Reports

| ID | Módulo | Tipo | Severidade | Status |
|---|---|---|---|---|
| BUG-001 | Cotação / Ordem de Compra | Validação de campo | Alta | Aberto |
| BUG-002 | Consulta geral | Performance | Alta | Aberto |
| BUG-003 | Anexos | Funcional | Alta | Aberto |
| BUG-004 | Busca por fornecedor | Funcional | Média | Aberto |
| BUG-005 | Integração portal externo | Integração | Crítica | Aberto |
| BUG-006 | Aditivos de contrato | Funcional | Crítica | Aberto |
