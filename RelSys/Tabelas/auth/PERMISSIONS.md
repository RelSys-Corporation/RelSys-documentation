| NOME       | TIPO         | NULO | CONSTRAINT             |
| :--------- | ------------ | ---- | ---------------------- |
| ID         | BIGINT       | NÃO  | PK_PERMISSIONS_ID      |
| NAME       | VARCHAR(100) | NÃO  | UQ_PERMISSIONS_TO_NAME |
| CREATED_AT | TIMESTAMP    | NÃO  |                        |
| UPDATED_AT | TIMESTAMP    | SIM  |                        |
