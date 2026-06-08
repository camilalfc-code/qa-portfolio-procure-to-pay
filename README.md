# 📦 QA Portfolio — Procure-to-Pay (P2P)

> Projeto de portfólio de Quality Assurance aplicado ao ciclo completo de compras corporativas — do pedido ao pagamento.

---

## 📌 Sobre o Projeto

Este repositório documenta a estratégia de testes para um **Sistema de Gestão de Compras e Contratos**, cobrindo o fluxo **Procure-to-Pay (P2P)** — modelo utilizado por ERPs corporativos como SAP, TOTVS e Oracle para gerenciar o ciclo completo de aquisições.

O projeto simula um ambiente real de QA, incluindo planejamento de testes, casos de teste manuais, cenários de exceção baseados em regras de negócio e testes de API.

---

## 🔄 Fluxo Procure-to-Pay Coberto

```
Solicitação → Cotação → Processo de Compra → Contrato/ARP → 
Empenho (PO) → Entrega → Nota Fiscal → Liquidação → Pagamento
```

### Tipos de Aquisição

| Tipo | Descrição |
|---|---|
| **Aquisição** | Compra de materiais e insumos |
| **Serviços** | Contratação de prestadores |
| **Obras** | Contratos de engenharia e reforma |
| **Locação** | Aluguel de espaços e equipamentos |

---

## 📁 Estrutura do Repositório

```
qa-portfolio-procure-to-pay/
│
├── docs/
│   ├── plano-de-testes.md          # Estratégia geral de testes
│   └── regras-de-negocio.md        # Regras e validações do sistema
│
├── casos-de-teste/
│   ├── CT-001_solicitacao.md       # Testes de solicitação de compra
│   ├── CT-002_cotacao.md           # Testes de cotação de preços
│   ├── CT-003_contrato-arp.md      # Testes de contrato e ARP
│   ├── CT-004_empenho.md           # Testes de empenho / ordem de compra
│   ├── CT-005_entrega.md           # Testes de recebimento de mercadoria
│   └── CT-006_pagamento.md         # Testes de nota fiscal e pagamento
│
├── cenarios-de-excecao/
│   ├── EXC-001_atraso-entrega.md   # Fornecedor não entrega no prazo
│   ├── EXC-002_entrega-parcial.md  # Entrega incompleta
│   ├── EXC-003_nota-fiscal.md      # NF com valor ou dados incorretos
│   └── EXC-004_contrato-vencido.md # Contrato expirado sem renovação
│
├── bug-reports/
│   └── (a ser preenchido com bugs encontrados)
│
└── postman/
    └── (coleção de testes de API REST)
```

---

## 🧪 Casos de Teste — Resumo

| ID | Módulo | Tipo | Status |
|---|---|---|---|
| CT-001 | Solicitação de Compra | Manual | ✅ Documentado |
| CT-002 | Cotação de Preços | Manual | ✅ Documentado |
| CT-003 | Contrato / ARP | Manual | ✅ Documentado |
| CT-004 | Empenho / Purchase Order | Manual | ✅ Documentado |
| CT-005 | Recebimento de Mercadoria | Manual | ✅ Documentado |
| CT-006 | Nota Fiscal e Pagamento | Manual | ✅ Documentado |
| EXC-001 | Atraso de Entrega | Exceção | ✅ Documentado |
| EXC-002 | Entrega Parcial | Exceção | ✅ Documentado |
| EXC-003 | Nota Fiscal Inválida | Exceção | ✅ Documentado |
| EXC-004 | Contrato Vencido | Exceção | ✅ Documentado |

---

## 🎯 Habilidades Demonstradas

- Análise de requisitos e regras de negócio
- Elaboração de plano de testes
- Escrita de casos de teste manuais (fluxo principal e exceções)
- Identificação de cenários de borda (*edge cases*)
- Documentação de bug reports
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
