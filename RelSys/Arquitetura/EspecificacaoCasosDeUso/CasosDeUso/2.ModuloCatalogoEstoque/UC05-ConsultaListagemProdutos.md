
UC05 - Consulta e Listagem de Produtos Objetivo: Buscar e visualizar itens do inventário. Requisitos Relacionados: [RF06, RF09] 

Fluxo Principal (Visualizar e Buscar Produto):

1. O usuário acessa "Produtos" > "Listagem" para consultar a posição atual [RF09].
    
2. A tabela exibe ID, Nome, Código de Barras e Preço.
    
3. O usuário digita no campo "Buscar produto" (nome ou código de barras) [RF06].
    
4. O sistema filtra automaticamente e exibe as correspondências.
    

Fluxos Alternativos:

- FA01 - Busca Sem Resultados: O termo não existe no banco. O sistema oculta a tabela e exibe a mensagem (MSG12).
    
- FA02 - Novo Produto: O usuário clica em "+ Novo Produto" e é redirecionado ao UC04.
    

Mensagens do Sistema (UC05):

- MSG12: "Nenhum produto encontrado. Tente alterar o termo de busca."
    
    

```mermaid-next
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    User->>UI: Acessa "Produtos" > "Listagem" [RF09]
    UI->>API: GET /api/v1/products
    API->>DB: Consulta estoque atual
    DB-->>API: Retorna produtos
    API-->>UI: Exibe tabela (ID, Nome, Código de Barras, Preço)

    alt Fluxo Principal: Buscar Produto [RF06]
        User->>UI: Digita no campo "Buscar produto" (Nome ou Código)
        UI->>API: GET /api/v1/products/search?q={termo}
        API->>DB: Filtra produtos por Nome ou Código de Barras
        DB-->>API: Retorna itens correspondentes
        API-->>UI: Exibe produtos filtrados na tabela
    else FA01: Busca Sem Resultados
        User->>UI: Digita termo inexistente
        UI->>API: GET /api/v1/products/search?q={termo_inexistente}
        API->>DB: Filtra produtos
        DB-->>API: Nenhum registro encontrado
        API-->>UI: Lista vazia
        UI-->>User: Oculta tabela e exibe MSG12
    else FA02: Redirecionar para Novo Produto
        User->>UI: Clica em "+ Novo Produto"
        UI-->>User: Redireciona para formulário de cadastro (UC04)
    end
```