| NOME                | TIPO        | NULO | CONSTRAINT                                |
| :------------------ | ----------- | ---- | ----------------------------------------- |
| ID                  | BIGINT      | NÃO  | PK_PHYSICAL_PERSON_TO_ID                  |
| PERSON_ID           | BIGINT      | NÃO  | FK_PHYSICAL_PERSON_TO_PERSON_ID           |
| PERSON_TYPE_ACRONYM | VARCHAR(2)  | NÃO  | FK_PHYSICAL_PERSON_TO_PERSON_TYPE_ACRONYM |
| CPF                 | VARCHAR(11) | NÃO  | UQ_PHYSICAL_PERSON_TO_CPF                 |
| CREATED_AT          | TIMESTAMP   | NÃO  |                                           |
| UPDATED_AT          | TIMESTAMP   | SIM  |                                           |