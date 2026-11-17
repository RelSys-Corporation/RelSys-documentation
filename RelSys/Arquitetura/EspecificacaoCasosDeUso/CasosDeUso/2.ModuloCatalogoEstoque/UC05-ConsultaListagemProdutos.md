
Objetivo: Buscar e visualizar itens do inventário, saldos e lotes. Requisitos Relacionados: [RF06, RF09] 

Fluxo Principal (Visualizar e Buscar Produto):

1. O usuário acessa "Produtos" > "Listagem" para consultar a posição atual [RF09].
    
2. A tabela exibe ID, Nome, Código de Barras, Preço, Saldo Total e indicador de validade dos lotes ativos.
    
3. O usuário digita no campo "Buscar produto" (nome ou código de barras) [RF06].
    
4. O sistema filtra automaticamente e exibe as correspondências.
    

Fluxos Alternativos:

- FA01 - Busca Sem Resultados: O termo não existe no banco. O sistema oculta a tabela e exibe a mensagem (MSG12).
    
- FA02 - Novo Produto: O usuário clica em "+ Novo Produto" e é redirecionado ao UC04.
    

Mensagens do Sistema (UC05):

- MSG12: "Nenhum produto encontrado. Tente alterar o termo de busca."
    

**

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    User->>UI: Acessa "Produtos" > "Listagem" [RF09]
    UI->>API: GET /api/v1/products
    API->>DB: Consulta produtos, saldos totais e validade dos lotes ativos
    DB-->>API: Retorna inventário
    API-->>UI: Renderiza tabela (ID, Nome, Cód. Barras, Preço, Saldo, Validade)

    alt Fluxo Principal: Buscar Produto
        User->>UI: Digita no campo "Buscar produto" (nome ou cód. barras) [RF06]
        UI->>API: GET /api/v1/products?search={termo}
        API->>DB: Filtra correspondências
        DB-->>API: Retorna produtos filtrados
        API-->>UI: Atualiza tabela com correspondências
    else FA01: Busca Sem Resultados
        User->>UI: Digita termo inexistente
        UI->>API: GET /api/v1/products?search={termo}
        API->>DB: Consulta termo
        DB-->>API: Lista vazia
        API-->>UI: Sem resultados
        UI-->>User: Oculta tabela e exibe MSG12
    else FA02: Novo Produto
        User->>UI: Clica em "+ Novo Produto"
        UI-->>User: Redireciona para UC04
    end
```