| NOME          | TIPO      | NULO | CONSTRAINT                             |
| :------------ | --------- | ---- | -------------------------------------- |
| ID            | BIGINT    | NÃO  | PK_ROLES_PERMISSIONS_ID                |
| ROLE_ID       | BIGINT    | NÃO  | FK_ROLES_PERMISSIONS_TO_ROLES_ID       |
| PERMISSION_ID | BIGINT    | NÃO  | FK_ROLES_PERMISSIONS_TO_PERMISSIONS_ID |
| CREATED_AT    | TIMESTAMP | NÃO  |                                        |
| UPDATED_AT    | TIMESTAMP | SIM  |                                        |
