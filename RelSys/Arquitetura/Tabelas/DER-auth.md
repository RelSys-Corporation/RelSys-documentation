```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
erDiagram
    PERMISSIONS_ENTITIES ||--o{ PERMISSIONS : "agrupa"
    ROLES ||--o{ ROLES_PERMISSIONS : "concede"
    PERMISSIONS ||--o{ ROLES_PERMISSIONS : "vinculada em"
    ROLES ||--o{ USERS : "atribui"
    USERS ||--o{ USERS_COMPANY : "acessa"

    PERMISSIONS_ENTITIES {
        bigint ID PK
        varchar NAME UK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    PERMISSIONS {
        bigint ID PK
        bigint PERMISSION_ENTITY_ID FK
        varchar METHOD_NAME
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    ROLES {
        bigint ID PK
        varchar NAME UK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    ROLES_PERMISSIONS {
        bigint ID PK
        bigint ROLE_ID FK
        bigint PERMISSION_ID FK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    USERS {
        bigint ID PK
        varchar NAME UK
        varchar PASSWORD
        bigint ROLE_ID FK
        boolean ACTIVE
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    USERS_COMPANY {
        bigint ID PK
        bigint USER_ID FK
        bigint COMPANY_ID FK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }
```

## Tabelas

| Tabela | Descrição |
|---|---|
| `PERMISSIONS_ENTITIES` | Entidades que podem receber permissões (ex.: `PRODUCT`, `SALE`, `ROLE`). |
| `PERMISSIONS` | Permissão = entidade + método HTTP (`METHOD_NAME` ∈ `GET`, `GET_ID`, `POST`, `PUT`). |
| `ROLES` | Papéis de acesso (ex.: `ADMIN`). |
| `ROLES_PERMISSIONS` | Associação N:N entre `ROLES` e `PERMISSIONS` (par `ROLE_ID`, `PERMISSION_ID` único). |
| `USERS` | Usuário do sistema; pertence a um `ROLE`. |
| `USERS_COMPANY` | Associação N:N entre `USERS` e `SYSTEM.COMPANY` (par `USER_ID`, `COMPANY_ID` único). |

## Relacionamentos internos

| Origem | Cardinalidade | Destino | Coluna(s) |
|---|---|---|---|
| `PERMISSIONS_ENTITIES` | 1 : 0..N | `PERMISSIONS` | `PERMISSION_ENTITY_ID` |
| `ROLES` | 1 : 0..N | `ROLES_PERMISSIONS` | `ROLE_ID` |
| `PERMISSIONS` | 1 : 0..N | `ROLES_PERMISSIONS` | `PERMISSION_ID` |
| `ROLES` | 1 : 0..N | `USERS` | `ROLE_ID` |
| `USERS` | 1 : 0..N | `USERS_COMPANY` | `USER_ID` |

## Referências externas

| Coluna | Refere a |
|---|---|
| `USERS_COMPANY.COMPANY_ID` | `SYSTEM.COMPANY.ID` |
