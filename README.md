# 📦 QA Portfolio — Procure-to-Pay (P2P)

> Projeto de portfólio de Quality Assurance aplicado ao ciclo completo de compras corporativas — do pedido ao pagamento.

---

## 📌 Sobre o Projeto

Este repositório documenta a estratégia de testes para um **Sistema de Gestão de Compras e Contratos**, cobrindo o fluxo **Procure-to-Pay (P2P)** — modelo utilizado por ERPs corporativos como SAP, TOTVS e Oracle para gerenciar o ciclo completo de aquisições.

O projeto simula um ambiente real de QA, incluindo planejamento de testes, casos de teste manuais, cenários de exceção baseados em regras de negócio, bug reports e testes de API.

---

## 🔄 Fluxo Procure-to-Pay Coberto

```
Requisição → Cotação → Aprovação → Ordem de Compra (PO) → 
Entrega → Nota Fiscal → Liquidação → Pagamento
```

### Tipos de Aquisição

| Tipo | Descrição |
|---|---|
| **Aquisição** | Compra de materiais e insumos |
| **Serviços** | Contratação de prestadores e mão de obra |
| **Obras** | Contratos de engenharia e reforma |
| **Locação** | Aluguel de espaços e equipamentos |

---

## 📁 Estrutura do Repositório

```
qa-portfolio-procure-to-pay/
│
├── casos-de-teste/
│   ├── CT-001_solicitacao.md               # Testes de requisição de compra
│   ├── CT-002_cotacao.md                   # Testes de cotação de preços
│   ├── CT-003_ordem-de-compra.md           # Testes de ordem de compra (PO)
│   ├── CT-004_recebimento-nf-pagamento.md  # Testes de recebimento, NF e pagamento
│   └── CT-005_contratos-servico.md         # Testes de contratos de serviço
│
├── bug-reports/
│   └── BUG-REPORTS.md                      # Bug reports documentados
│
└── postman/
    └── (coleção de testes de API REST)
```

---

## 🧪 Casos de Teste — Resumo

| ID | Módulo | Tipo | Casos | Status |
|---|---|---|---|---|
| CT-001 | Requisição de Compra | Manual | 9 | ✅ Documentado |
| CT-002 | Cotação de Preços | Manual | 9 | ✅ Documentado |
| CT-003 | Ordem de Compra (PO) | Manual | 9 | ✅ Documentado |
| CT-004 | Recebimento, NF e Pagamento | Manual | 10 | ✅ Documentado |
| CT-005 | Contratos de Serviço | Manual | 10 | ✅ Documentado |

**Total: 47 casos de teste documentados**

---

## 🐛 Bug Reports — Resumo

| ID | Módulo | Tipo | Severidade |
|---|---|---|---|
| BUG-001 | Cotação / Ordem de Compra | Validação de campo | Alta |
| BUG-002 | Consulta geral | Performance | Alta |
| BUG-003 | Anexos | Funcional | Alta |
| BUG-004 | Busca por fornecedor | Funcional | Média |
| BUG-005 | Integração portal externo | Integração | Crítica |
| BUG-006 | Aditivos de contrato | Funcional | Crítica |

**Total: 6 bug reports documentados**

---

## 🎯 Habilidades Demonstradas

- Análise de requisitos e regras de negócio
- Elaboração de plano de testes
- Escrita de casos de teste manuais (fluxo principal, alternativo e exceções)
- Identificação de cenários de borda (*edge cases*)
- Documentação de bug reports com severidade e evidências
- Testes de API REST com Postman
- Conhecimento de domínio em ciclo de compras corporativas (P2P)

---

## 🛠️ Ferramentas

![Postman](https://img.shields.io/badge/Postman-FF6C37?style=flat&logo=postman&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat&logo=mysql&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)
![Jira](https://img.shields.io/badge/Jira-0052CC?style=flat&logo=jira&logoColor=white)

---

## 👩‍💻 Autora

**Camila Lopes**  
QA Analyst | Testes Manuais | SQL | Postman | Cypress  
[LinkedIn](https://www.linkedin.com/in/camila-lopes-ferreira-carvalho43235429/) • [GitHub](https://github.com/camilalfc-code)

---

> *"Qualidade não é o que você coloca num produto. É o que o cliente tira dele."* — Peter Drucker
