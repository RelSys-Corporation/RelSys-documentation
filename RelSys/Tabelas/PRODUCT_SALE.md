| NOME                | TIPO           | NULO | CONSTRAINT                    |
| :------------------ | -------------- | ---- | ----------------------------- |
| ID                  | BIGINT         | NÃO  | PK_PRODUCT_SALE_ID            |
| SALE_ID             | BIGINT         | NÃO  | FK_PRODUCT_SALE_TO_SALE_ID    |
| PRODUCT_ID          | BIGINT         | NÃO  | FK_PRODUCT_SALE_TO_PRODUCT_ID |
| QUANTITY            | NUMERIC(15, 3) | NÃO  |                               |
| DISCOUNT_PERCENTAGE | NUMERIC(5, 2)  | NÃO  |                               |
| PRODUCT_NAME        | VARCHAR(100)   | NÃO  |                               |
| PRODUCT_PRICE       | NUMERIC(15, 3) | NÃO  |                               |
| CREATED_AT          | TIMESTAMP      | NÃO  |                               |
| UPDATED_AT          | TIMESTAMP      | SIM  |                               |