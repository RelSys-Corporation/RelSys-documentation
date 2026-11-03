
Objetivo: Buscar e visualizar itens do inventário. Requisitos Relacionados: [RF06, RF09]

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
    participant UI as Visão (Interface)
    participant Controller as Controlador (Produtos)
    participant DB as Banco de Dados

    User->>UI: Acessa "Produtos" > "Listagem" [RF09]
    UI->>Controller: listarProdutos()
    Controller->>DB: SELECT id, nome, codigo_barras, preco FROM produtos
    DB-->>Controller: listaProdutos
    Controller-->>UI: Exibe tabela (ID, Nome, Código, Preço)

    alt Fluxo Principal: Visualizar e Buscar Produto
        User->>UI: Digita termo no campo "Buscar produto" (nome/código) [RF06]
        UI->>Controller: buscarProdutos(termo)
        Controller->>DB: SELECT * FROM produtos WHERE nome LIKE %termo% OR codigo_barras LIKE %termo%
        DB-->>Controller: correspondencias
        Controller-->>UI: renderizaTabelaFiltro(correspondencias)
        UI-->>User: Exibe resultados filtrados

    else FA01 - Busca Sem Resultados
        User->>UI: Digita termo inexistente
        UI->>Controller: buscarProdutos(termoInexistente)
        Controller->>DB: SELECT ...
        DB-->>Controller: vazio
        Controller-->>UI: MSG12 ("Nenhum produto encontrado...")
        UI-->>User: Oculta tabela e exibe MSG12

    else FA02 - Novo Produto
        User->>UI: Clica em "+ Novo Produto"
        UI-->>User: Redireciona para tela de cadastro (UC04)
    end
```