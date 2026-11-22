```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
erDiagram
    PRODUCT ||--o{ PRODUCT_SUPPLIER : "negociado como"
    PRODUCT_SUPPLIER ||--o{ PRODUCT_BATCH : "gerado em"
    SALE_STATUS ||--o{ SALE : "classifica"
    SALE ||--o{ PRODUCT_SALE : "contém"
    PRODUCT ||--o{ PRODUCT_SALE : "vendido em"

    PRODUCT {
        bigint ID PK
        bigint COMPANY_ID FK
        varchar NAME UK
        numeric PRICE
        varchar BARCODE UK
        numeric MINIMUM_STOCK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    PRODUCT_SUPPLIER {
        bigint ID PK
        bigint COMPANY_ID FK
        bigint PRODUCT_ID FK
        bigint SUPPLIER_ID FK
        boolean DISCOUNT_ALLOWED
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    PRODUCT_BATCH {
        bigint ID PK
        bigint COMPANY_ID FK
        bigint PRODUCT_SUPPLIER_ID FK
        numeric PURCHASE_PRICE
        timestamp EXPIRATION_DATE
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    SALE_STATUS {
        bigint ID PK
        varchar NAME UK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    SALE {
        bigint ID PK
        bigint COMPANY_ID FK
        bigint CLIENT_ID FK
        bigint PAYMENT_METHOD_ID FK
        numeric DISCOUNT_PERCENTAGE
        bigint SALE_STATUS_ID FK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    PRODUCT_SALE {
        bigint ID PK
        bigint COMPANY_ID FK
        bigint SALE_ID FK
        bigint PRODUCT_ID FK
        numeric QUANTITY
        numeric DISCOUNT_PERCENTAGE
        varchar PRODUCT_NAME
        numeric PRODUCT_PRICE
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }
```

## Tabelas

| Tabela | Descrição |
|---|---|
| `PRODUCT` | Produto cadastrado para a empresa (`NAME` e `BARCODE` únicos por empresa). |
| `PRODUCT_SUPPLIER` | Associação N:N entre `PRODUCT` e `SUPPLIER` (par `PRODUCT_ID`, `SUPPLIER_ID` único por empresa). |
| `PRODUCT_BATCH` | Lote de compra de um produto de um fornecedor (gera os lotes financeiros/estoque). |
| `SALE_STATUS` | Catálogo de status de venda (`PROCESSED`, `CANCELED`). |
| `SALE` | Venda; `DISCOUNT_PERCENTAGE` entre 0 e 100. |
| `PRODUCT_SALE` | Item de venda (lança denormalizações `PRODUCT_NAME` e `PRODUCT_PRICE` no momento da venda). |

## Relacionamentos internos

| Origem | Cardinalidade | Destino | Coluna(s) |
|---|---|---|---|
| `PRODUCT` | 1 : 0..N | `PRODUCT_SUPPLIER` | `PRODUCT_ID` (+ `COMPANY_ID`) |
| `PRODUCT_SUPPLIER` | 1 : 0..N | `PRODUCT_BATCH` | `PRODUCT_SUPPLIER_ID` (+ `COMPANY_ID`) |
| `SALE_STATUS` | 1 : 0..N | `SALE` | `SALE_STATUS_ID` |
| `SALE` | 1 : 0..N | `PRODUCT_SALE` | `SALE_ID` (+ `COMPANY_ID`) |
| `PRODUCT` | 1 : 0..N | `PRODUCT_SALE` | `PRODUCT_ID` (+ `COMPANY_ID`) |

## Referências externas

> Todas as tabelas com `COMPANY_ID` referenciam `SYSTEM.COMPANY.ID`.

| Coluna | Refere a |
|---|---|
| `PRODUCT_SUPPLIER.SUPPLIER_ID` (+ `COMPANY_ID`) | `CRM.SUPPLIER.ID` |
| `SALE.PAYMENT_METHOD_ID` (+ `COMPANY_ID`) | `FINANCIAL.PAYMENT_METHOD.ID` |
| `SALE.CLIENT_ID` (+ `COMPANY_ID`) | `CRM.CLIENT.ID` |
