| NOME             | TIPO      | NULO | CONSTRAINT                         |
| :--------------- | --------- | ---- | ---------------------------------- |
| ID               | BIGINT    | NÃO  | PK_PRODUCT_SUPPLIER_ID             |
| PRODUCT_ID       | BIGINT    | NÃO  | FK_PRODUCT_SUPPLIER_TO_PRODUCT_ID  |
| SUPPLIER_ID      | BIGINT    | NÃO  | FK_PRODUCT_SUPPLIER_TO_SUPPLIER_ID |
| DISCOUNT_ALLOWED | BOOLEAN   | NÃO  |                                    |
| CREATED_AT       | TIMESTAMP | NÃO  |                                    |
| UPDATED_AT       | TIMESTAMP | SIM  |                                    |
