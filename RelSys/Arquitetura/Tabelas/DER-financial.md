```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
erDiagram
    PAYMENT_METHOD_TYPE ||--o{ PAYMENT_METHOD : "classifica"
    PAYMENT_METHOD ||--o{ CASH_PROCESS : "usado em"
    FINANCIAL_ACCOUNT ||--o{ FINANCIAL_TITLE : "origina"
    FINANCIAL_TITLE_BATCH ||--o{ FINANCIAL_TITLE : "agrupa"
    FINANCIAL_TITLE_STATUS ||--o{ FINANCIAL_TITLE : "classifica"
    FINANCIAL_TITLE_BATCH ||--o{ FINANCIAL_TITLE_ORIGIN : "relaciona a"
    CASH_PROCESS ||--o{ CASH_PROCESS_ITEM : "baixa"
    FINANCIAL_TITLE ||--o{ CASH_PROCESS_ITEM : "quitado em"
    CASH_PROCESS ||--o{ CASH_MOVEMENT : "gera"
    FINANCIAL_ACCOUNT ||--o{ CASH_MOVEMENT : "registrado em"

    PAYMENT_METHOD_TYPE {
        bigint ID PK
        varchar NAME UK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    PAYMENT_METHOD {
        bigint ID PK
        bigint COMPANY_ID FK
        varchar NAME UK
        varchar ACRONYM UK
        bigint PAYMENT_METHOD_TYPE_ID FK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    FINANCIAL_ACCOUNT {
        bigint ID PK
        bigint COMPANY_ID FK
        varchar NAME UK
        varchar FLUX_ACRONYM
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    FINANCIAL_TITLE_STATUS {
        bigint ID PK
        varchar NAME UK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    FINANCIAL_TITLE_BATCH {
        bigint ID PK
        bigint COMPANY_ID FK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    FINANCIAL_TITLE {
        bigint ID PK
        bigint COMPANY_ID FK
        bigint FINANCIAL_ACCOUNT_ID FK
        numeric INSTALLMENT
        bigint FINANCIAL_TITLE_BATCH_ID FK
        bigint PERSON_ID FK
        bigint FINANCIAL_TITLE_STATUS_ID FK
        numeric VALUE
        numeric PAYED_VALUE
        timestamp EXPIRATION_DATE
        timestamp FULL_PAY_DATE
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    FINANCIAL_TITLE_ORIGIN {
        bigint ID PK
        bigint COMPANY_ID FK
        bigint FINANCIAL_TITLE_BATCH_ID FK
        bigint SALE_ID FK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    CASH_PROCESS {
        bigint ID PK
        bigint COMPANY_ID FK
        bigint PAYMENT_METHOD_ID FK
        numeric TOTAL_VALUE
        timestamp PROCESS_DATE
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    CASH_PROCESS_ITEM {
        bigint ID PK
        bigint COMPANY_ID FK
        bigint CASH_PROCESS_ID FK
        bigint FINANCIAL_TITLE_ID FK
        numeric FINANCIAL_TITLE_INSTALLMENT
        numeric PAID_VALUE
        numeric DISCOUNT_VALUE
        numeric INTEREST_VALUE
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    CASH_MOVEMENT {
        bigint ID PK
        bigint COMPANY_ID FK
        bigint CASH_PROCESS_ID FK
        bigint FINANCIAL_ACCOUNT_ID FK
        varchar FLUX_ACRONYM
        numeric VALUE
        timestamp MOVEMENT_DATE
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }
```

## Tabelas

| Tabela | Descrição |
|---|---|
| `PAYMENT_METHOD_TYPE` | Catálogo de tipos de pagamento (`SINGLE`, `DEFERRED`). |
| `PAYMENT_METHOD` | Método de pagamento da empresa (`NAME` e `ACRONYM` únicos por empresa). |
| `FINANCIAL_ACCOUNT` | Conta financeira (fluxo `R` recebível ou `P` pagável). |
| `FINANCIAL_TITLE_STATUS` | Catálogo de status (`PENDING`, `PARTIAL`, `PAID`, `CANCELED`). |
| `FINANCIAL_TITLE_BATCH` | Lote de títulos gerados em um mesmo evento (ex.: uma venda dividida em parcelas). |
| `FINANCIAL_TITLE` | Título financeiro (parcela); `INSTALLMENT` indica o número da parcela. |
| `FINANCIAL_TITLE_ORIGIN` | Origem do título (ex.: `SALE` do `COMMERCIAL`). |
| `CASH_PROCESS` | Processo de caixa (fechamento/lançamento de pagamentos). |
| `CASH_PROCESS_ITEM` | Baixa de um título dentro do processo de caixa (com desconto/juros). |
| `CASH_MOVEMENT` | Movimentação financeira gerada pelo processo em uma conta (`FLUX_ACRONYM` ∈ `I`, `O`). |

## Relacionamentos internos

| Origem | Cardinalidade | Destino | Coluna(s) |
|---|---|---|---|
| `PAYMENT_METHOD_TYPE` | 1 : 0..N | `PAYMENT_METHOD` | `PAYMENT_METHOD_TYPE_ID` |
| `PAYMENT_METHOD` | 1 : 0..N | `CASH_PROCESS` | `PAYMENT_METHOD_ID` (+ `COMPANY_ID`) |
| `FINANCIAL_ACCOUNT` | 1 : 0..N | `FINANCIAL_TITLE` | `FINANCIAL_ACCOUNT_ID` (+ `COMPANY_ID`) |
| `FINANCIAL_TITLE_BATCH` | 1 : 0..N | `FINANCIAL_TITLE` | `FINANCIAL_TITLE_BATCH_ID` (+ `COMPANY_ID`) |
| `FINANCIAL_TITLE_STATUS` | 1 : 0..N | `FINANCIAL_TITLE` | `FINANCIAL_TITLE_STATUS_ID` |
| `FINANCIAL_TITLE_BATCH` | 1 : 0..N | `FINANCIAL_TITLE_ORIGIN` | `FINANCIAL_TITLE_BATCH_ID` (+ `COMPANY_ID`) |
| `CASH_PROCESS` | 1 : 0..N | `CASH_PROCESS_ITEM` | `CASH_PROCESS_ID` (+ `COMPANY_ID`) |
| `FINANCIAL_TITLE` | 1 : 0..N | `CASH_PROCESS_ITEM` | `FINANCIAL_TITLE_ID` + `FINANCIAL_TITLE_INSTALLMENT` (+ `COMPANY_ID`) |
| `CASH_PROCESS` | 1 : 0..N | `CASH_MOVEMENT` | `CASH_PROCESS_ID` (+ `COMPANY_ID`) |
| `FINANCIAL_ACCOUNT` | 1 : 0..N | `CASH_MOVEMENT` | `FINANCIAL_ACCOUNT_ID` (+ `COMPANY_ID`) |

## Referências externas

> Todas as tabelas com `COMPANY_ID` referenciam `SYSTEM.COMPANY.ID`.

| Coluna | Refere a |
|---|---|
| `FINANCIAL_TITLE.PERSON_ID` (+ `COMPANY_ID`) | `CRM.PERSON.ID` |
| `FINANCIAL_TITLE_ORIGIN.SALE_ID` (+ `COMPANY_ID`) | `COMMERCIAL.SALE.ID` |
