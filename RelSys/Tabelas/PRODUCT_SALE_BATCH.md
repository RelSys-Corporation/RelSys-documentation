| NOME             | TIPO           | NULO | CONSTRAINT                                |
| :--------------- | -------------- | ---- | ----------------------------------------- |
| ID               | BIGINT         | NÃO  | PK_PRODUCT_SALE_BATCH_ID                  |
| PRODUCT_SALE_ID  | BIGINT         | NÃO  | FK_PRODUCT_SALE_BATCH_TO_PRODUCT_SALE_ID  |
| PRODUCT_BATCH_ID | BIGINT         | NÃO  | FK_PRODUCT_SALE_BATCH_TO_PRODUCT_BATCH_ID |
| QUANTITY         | NUMERIC(15, 3) | NÃO  |                                           |
| APPLY_DISCOUNT   | BOOLEAN        | NÃO  |                                           |
| PURCHASE_PRICE   | NUMERIC(15, 3) | NÃO  |                                           |
| CREATED_AT       | TIMESTAMP      | NÃO  |                                           |
| UPDATED_AT       | TIMESTAMP      | SIM  |                                           |
