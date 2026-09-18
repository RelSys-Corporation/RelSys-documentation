| NOME             | TIPO      | NULO | CONSTRAINT                    |
| :--------------- | --------- | ---- | ----------------------------- |
| ID               | BIGINT    | NÃO  | PK_CLIENT_ID                  |
| PERSON_ID        | BIGINT    | NÃO  | FK_CLIENT_TO_PERSON_ID        |
| PERSON_STATUS_ID | BIGINT    | NÃO  | FK_CLIENT_TO_PERSON_STATUS_ID |
| CREATED_AT       | TIMESTAMP | NÃO  |                               |
| UPDATED_AT       | TIMESTAMP | SIM  |                               |

