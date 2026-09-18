| NOME                | TIPO          | NULO | CONSTRAINT                   |
| :------------------ | ------------- | ---- | ---------------------------- |
| ID                  | BIGINT        | NÃO  | PK_SALE_ID                   |
| CLIENT_ID           | BIGINT        | SIM  | FK_SALE_TO_CLIENT_ID         |
| PAYMENT_METHOD_ID   | BIGINT        | NÃO  | FK_SALE_TO_PAYMENT_METHOD_ID |
| DISCOUNT_PERCENTAGE | NUMERIC(5, 2) | NÃO  |                              |
| SALE_STATUS_ID      | BIGINT        | NÃO  | FK_SALE_TO_SALE_STATUS_ID    |
| CREATED_AT          | TIMESTAMP     | NÃO  |                              |
| UPDATED_AT          | TIMESTAMP     | SIM  |                              |