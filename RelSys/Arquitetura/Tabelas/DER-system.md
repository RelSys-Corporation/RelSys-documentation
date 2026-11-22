```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
erDiagram
    COMPANY |o--o{ COMPANY : "filial da (auto-relacionamento)"

    MESSAGE {
        bigint ID PK
        varchar NAME UK
        varchar CONTENT
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    MESSAGE_ENTITY {
        bigint ID PK
        varchar NAME UK
        varchar LABEL UK
        varchar ARTICLE
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    MESSAGE_PROPERTY {
        bigint ID PK
        varchar NAME UK
        varchar LABEL UK
        varchar ARTICLE
        varchar SUFFIX
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }

    COMPANY {
        bigint ID PK
        varchar CORPORATE_NAME UK
        varchar TRADE_NAME
        boolean IS_HEADQUARTER
        bigint HEADQUARTER_COMPANY_ID FK
        timestamp CREATED_AT
        timestamp UPDATED_AT
    }
```

## Tabelas

| Tabela | Descrição |
|---|---|
| `MESSAGE` | Catálogo de mensagens de erro/regra de negócio (`NAME` único). |
| `MESSAGE_ENTITY` | Catálogo de entidades referenciadas nas mensagens (`NAME` e `LABEL` únicos). |
| `MESSAGE_PROPERTY` | Catálogo de propriedades referenciadas nas mensagens (`NAME` e `LABEL` únicos). |
| `COMPANY` | Empresa; `IS_HEADQUARTER` indica matriz. Pode apontar (`HEADQUARTER_COMPANY_ID`) para a empresa matriz. |

## Relacionamentos internos

| Origem | Cardinalidade | Destino | Coluna(s) |
|---|---|---|---|
| `COMPANY` (matriz) | 1 : 0..N | `COMPANY` (filial) | `HEADQUARTER_COMPANY_ID` |

## Referências externas

Nenhuma. Todas as chaves estrangeiras deste schema apontam para tabelas do próprio `SYSTEM`.
