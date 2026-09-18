| NOME                | TIPO           | NULO | CONSTRAINT                              |
| :------------------ | -------------- | ---- | --------------------------------------- |
| ID                  | BIGINT         | NÃO  | PK_PRODUCT_BATCH_ID                     |
| PRODUCT_SUPPLIER_ID | BIGINT         | NÃO  | FK_PRODUCT_BATCH_TO_PRODUCT_SUPPLIER_ID |
| PURCHASE_PRICE      | NUMERIC(15, 3) | NÃO  | CK_PRODUCT_BATCH_TO_PURCHASE_PRICE      |
| PURCHASE_QUANTITY   | NUMERIC(15, 3) | NÃO  | CK_PRODUCT_BATCH_TO_PURCHASE_QUANTITY   |
| REMAINING_QUANTITY  | NUMERIC(15, 3) | NÃO  | CK_PRODUCT_BATCH_TO_REMAINING_QUANTITY  |
| CREATED_AT          | TIMESTAMP      | NÃO  |                                         |
| UPDATED_AT          | TIMESTAMP      | SIM  |                                         |
