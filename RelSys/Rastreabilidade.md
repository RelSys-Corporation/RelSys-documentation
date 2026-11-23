# Rastreabilidade

## 1. Objetivo

Permitir percorrer o caminho **Requisito/Regra → Caso de Uso → Artefato de Modelo/API → Teste**, e o caminho inverso, durante a banca e a implementação.

## 2. Matriz Requisito → Caso de Uso → Artefato → Teste

| ID | Casos de uso | Modelo / documento | Teste de referência |
|---|---|---|---|
| RF01 | UC15, UC16 | `DER-auth`, `classDiagram-auth`, [[SegurancaPermissoes]] | T-SEC-01, T-SEC-02, T-SEC-03 |
| RF02 | UC02 | `DER-crm`, `classDiagram-crm`, [[Privacidade]] | T-CRM-01, T-CRM-02 |
| RF03 | UC03 | `DER-crm`, `classDiagram-crm` | T-CRM-03 |
| RF04 | UC01 | `DER-financial`, `classDiagram-financial` | T-FIN-01 |
| RF05 | UC04, UC05 | `DER-commercial`, `classDiagram-commercial` | T-COM-01 |
| RF06 | UC05, UC09 | `DER-commercial`, `classDiagram-commercial` | T-COM-02, T-PDV-01 |
| RF07 | UC04 | `classDiagram-shared` (`Barcode`) | T-COM-03 |
| RF08 | UC06 | `Barcode`, [[DiagramaIntegracao]] | T-COM-04 |
| RF09 | UC05, UC14 | `DER-stock`, `DER-commercial` | T-EST-01 |
| RF10 | UC07 | `DER-stock`, `classDiagram-stock` | T-EST-02 |
| RF11 | UC07 | `PRODUCT_BATCH`, `STOCK_MOVEMENT` | T-EST-03 |
| RF12 | UC08, UC09 | `SALE`, [[DiagramaEstados]] | T-PDV-02 |
| RF13 | UC11 | `SALE.DISCOUNT_PERCENTAGE`, `PRODUCT_SALE.DISCOUNT_PERCENTAGE` | T-PDV-03 |
| RF14 | UC01, UC11 | `PAYMENT_METHOD`, `SALE.PAYMENT_METHOD_ID` | T-PDV-04 |
| RF15 | UC10 | [[DiagramaIntegracao]] | T-PDV-05 |
| RF16 | UC08 | `CASH_PROCESS`, `CASH_MOVEMENT` (ver ressalva R-05) | T-CAI-01 |
| RF17 | UC12, UC13 | `SALE_STATUS`, [[DiagramaEstados]] (ver R-06) | T-PDV-06 |
| RF18 | UC14 | [[DiagramaComponentes]] | T-GER-01 |
| RF19 | UC13 | `SALE`, `PRODUCT_SALE` | T-GER-02 |
| RF20 | UC13 | [[ContratoAPI]] (`/reports/sales/export-pdf`) | T-GER-03 |

## 3. Matriz Regra de Negócio → Evidência

| ID | Regra | Casos de uso | Evidência / teste |
|---|---|---|---|
| RN01 | Isenção de desconto por produto/fornecedor | UC03, UC04, UC11 | `PRODUCT_SUPPLIER.DISCOUNT_ALLOWED`; lacuna em produto/fornecedor → R-03, R-08 |
| RN02 | PEPS/FEFO e custo por lote | UC07, UC11, UC12 | `PRODUCT_BATCH.EXPIRATION_DATE`, `STOCK_MOVEMENT`, `PRODUCT_SALE_BATCH`; lacuna R-02, R-09 |
| RN03 | Unicidade de código de barras por empresa | UC04, UC05 | `PRODUCT.BARCODE UK`, `Barcode.generateBarcode` — T-COM-03 |
| RN04 | Histórico imutável e não exclusão | UC02, UC11, UC12, UC13, UC16 | `SALE_STATUS`, metadados de cancelamento em aberto → R-06 |
| RN05 | Menor privilégio | UC12, UC14, UC15, UC16 | [[SegurancaPermissoes]] — R-07 |
| RN06 | Divergências de caixa por operação | UC08 | `CASH_PROCESS`, `CASH_MOVEMENT`; sessão em aberto → R-05 |

## 4. Matriz Requisito Não Funcional → Evidência

| ID | Casos de uso | Documento de apoio |
|---|---|---|
| RNF01 | UC10 | [[DiagramaIntegracao]] (impressora 58/80 mm) |
| RNF02 | UC06 | [[DiagramaIntegracao]] (ZPL/PPLA e PDF A4) |
| RNF03 | UC02, UC16 | [[Privacidade]], [[SegurancaPermissoes]] |
| RNF04 | UC14, UC15, UC16 | [[SegurancaPermissoes]] |
| RNF05 | UC02, UC08, UC13, UC14, UC15, UC16 | [[Arquitetura]], [[SegurancaPermissoes]] |

## 5. Rastreabilidade de casos de uso não representados no diagrama de UC

| Caso de uso | Situação | Ação |
|---|---|---|
| UC15 — Gerenciar Perfis e Permissões | Arquivo existe e é referenciado por RF01/RN05, mas não aparece como nó em [[Diagrama-UC]] | Registrado como R-11 em [[RiscosQuestoesAbertas]] |
| UC16 — Gerenciar Usuários | Idem | Registrado como R-11 |

## 6. Convenção de nomenclatura dos testes

- `T-SEC-xx` — segurança, permissões e isolamento
- `T-CRM-xx` — pessoas, clientes e fornecedores
- `T-COM-xx` — produtos, código de barras e etiquetas
- `T-EST-xx` — estoque e lotes
- `T-PDV-xx` — vendas, descontos, pagamento e cancelamento
- `T-CAI-xx` — sessão de caixa e divergências
- `T-GER-xx` — relatórios e dashboard

> Os testes listados são identificadores planejados; a evidência de execução deve ser anexada conforme [[QualidadeValidacao]] e [[ControleDocumental]].
