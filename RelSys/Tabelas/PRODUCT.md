| NOME       | TIPO           | NULO | CONSTRAINT            |
| :--------- | -------------- | ---- | --------------------- |
| ID         | BIGINT         | NÃO  | PK_PRODUCT_ID         |
| NAME       | BIGINT         | NÃO  | UQ_PRODUCT_TO_NAME    |
| PRICE      | NUMERIC(15, 3) | NÃO  |                       |
| BARCODE    | VARCHAR(50)    | SIM  | UQ_PRODUCT_TO_BARCODE |
| CREATED_AT | TIMESTAMP      | NÃO  |                       |
| UPDATED_AT | TIMESTAMP      | SIM  |                       |
