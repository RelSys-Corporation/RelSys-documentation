| NOME                | TIPO         | NULO | CONSTRAINT                |
| :------------------ | ------------ | ---- | ------------------------- |
| ID                  | BIGINT       | NÃO  | PK_PERSON_ID              |
| NAME                | VARCHAR(200) | NÃO  |                           |
| EMAIL               | VARCHAR(255) | SIM  | UQ_PERSON_TO_EMAIL        |
| PERSON_TYPE_ACRONYM | VARCHAR(2)   | SIM  | CK_PERSON_TO_TYPE_ACRONYM |
| CREATED_AT          | TIMESTAMP    | NÃO  |                           |
| UPDATED_AT          | TIMESTAMP    | SIM  |                           |