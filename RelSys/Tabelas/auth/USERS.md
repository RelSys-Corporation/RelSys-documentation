| NOME       | TIPO         | NULO | CONSTRAINT           |
| :--------- | ------------ | ---- | -------------------- |
| ID         | BIGINT       | NÃO  | PK_USERS_ID          |
| NAME       | VARCHAR(100) | NÃO  |                      |
| ROLE_ID    | BIGINT       | NÃO  | FK_USERS_TO_ROLES_ID |
| ACTIVE     | BOOLEAN      | NÃO  |                      |
| CREATED_AT | TIMESTAMP    | NÃO  |                      |
| UPDATED_AT | TIMESTAMP    | SIM  |                      |
