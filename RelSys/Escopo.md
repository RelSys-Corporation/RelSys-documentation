# Escopo

## 1. Objetivo

Delimitar o que faz e o que não faz parte da entrega v1 (primeira banca) do RelSys, evitando interpretações divergentes entre requisitos, casos de uso e modelos.

## 2. Escopo incluído

| Módulo | Funcionalidades compreendidas | Referência |
|---|---|---|
| Segurança e Acesso | Usuários, perfis, permissões, autenticação e isolamento por filial | RF01, RN05, RNF04, RNF05, UC15, UC16 |
| Cadastros e Pessoas | Clientes e fornecedores, pessoa física (CPF) e jurídica (CNPJ), status | RF02, RF03, UC02, UC03 |
| Estoque e Produtos | Produto, código de barras, etiquetas, consulta de saldos e validades | RF05–RF09, UC04–UC06 |
| Compras e Entradas | Recebimento de mercadorias com preço de compra, quantidade e validade por lote | RF10, RF11, UC07 |
| Vendas e Caixa | PDV, descontos, forma de pagamento, finalização, comprovante, sessão de caixa, cancelamento e estorno | RF12–RF17, UC08–UC12 |
| Relatórios e Gerenciamento | Histórico de vendas, exportação PDF e dashboard gerencial | RF18–RF20, UC13, UC14 |

## 3. Fora do escopo da v1

| Item | Justificativa |
|---|---|
| Emissão de NF-e/NFC-e e integração com SEFAZ | Cupom é descrito explicitamente como **não fiscal** (RF15). |
| Integração com adquirentes, bandeiras ou PIX automatizado | A v1 registra a forma de pagamento escolhida; não autoriza transações externas. |
| E-commerce, aplicativo móvel de cliente e portal do fornecedor | Não há ator, caso de uso ou modelo de dados correspondente. |
| Múltiplos warehouses com transferência entre unidades operacionais | O modelo possui `WAREHOUSE`/`WAREHOUSE_LOCATION`, mas os UCs de v1 não descrevem fluxo de transferência. |
| Cobrança, contas a pagar e conciliação bancária automática | `FINANCIAL_TITLE` existe, mas apenas como decorrência de vendas; não há fluxo de contas a pagar na v1. |
| Auditoria imutável em blockchain ou storage append-only externo | Imutabilidade tratada como regra de negócio (RN04) sobre o banco relacional. |
| Migração de dados legados | Não há fonte de dados anterior declarada. |

## 4. Premissas

- PostgreSQL é o banco relacional de referência, conforme `Apresentacao.md`.
- O isolamento por empresa/filial é lógico, via `COMPANY_ID`/`USERS_COMPANY`.
- O histórico de vendas é preservado; cancelamento altera status, não exclui registro (RN04).
- Impressoras térmicas e geração PDF são tratadas como integração local, detalhada em [[DiagramaIntegracao]].
- Os modelos Mermaid existentes são a fonte oficial para entidades, colunas, tipos e cardinalidades; eventuais lacunas são questões abertas, não alterações implícitas.

## 5. Critérios de saída do escopo

A entrega é considerada completa quando: todos os RF/RN possuem caso de uso correspondente ou registro de exceção; todos os UCs possuem fluxo principal, alternativo e diagrama de sequência; os artefatos obrigatórios e condicionais listados em [[00-Identificacao]] existem; e as divergências não resolvidas estão explicitamente listadas em [[RiscosQuestoesAbertas]].
