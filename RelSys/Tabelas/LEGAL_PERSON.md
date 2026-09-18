| NOME                | TIPO        | NULO | CONSTRAINT                             |
| :------------------ | ----------- | ---- | -------------------------------------- |
| ID                  | BIGINT      | NÃO  | PK_LEGAL_PERSON_ID                     |
| PERSON_ID           | BIGINT      | NÃO  | FK_LEGAL_PERSON_TO_PERSON_ID           |
| PERSON_TYPE_ACRONYM | VARCHAR(2)  | NÃO  | FK_LEGAL_PERSON_TO_PERSON_TYPE_ACRONYM |
| CNPJ                | VARCHAR(14) | NÃO  | UQ_LEGAL_PERSON_TO_CNPJ                |
| CREATED_AT          | TIMESTAMP   | NÃO  |                                        |
| UPDATED_AT          | TIMESTAMP   | SIM  |                                        |