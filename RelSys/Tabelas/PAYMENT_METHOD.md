| NOME       | TIPO        | NULO | CONSTRAINT                   |
| :--------- | ----------- | ---- | ---------------------------- |
| ID         | BIGINT      | NÃO  | PK_PAYMENT_METHOD_ID         |
| NAME       | VARCHAR(50) | NÃO  | UQ_PAYMENT_METHOD_TO_NAME    |
| ACRONYM    | VARCHAR(2)  | NÃO  | UQ_PAYMENT_METHOD_TO_ACRONYM |
| CREATED_AT | TIMESTAMP   | NÃO  |                              |
| UPDATED_AT | TIMESTAMP   | SIM  |                              |
