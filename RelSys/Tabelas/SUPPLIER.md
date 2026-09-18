| NOME             | TIPO      | NULO | CONSTRAINT                      |
| :--------------- | --------- | ---- | ------------------------------- |
| ID               | BIGINT    | NÃO  | PK_SUPPLIER_ID                  |
| PERSON_ID        | BIGINT    | NÃO  | FK_SUPPLIER_TO_PERSON_ID        |
| PERSON_STATUS_ID | BIGINT    | NÃO  | FK_SUPPLIER_TO_PERSON_STATUS_ID |
| CREATED_AT       | TIMESTAMP | NÃO  |                                 |
| UPDATED_AT       | TIMESTAMP | SIM  |                                 |