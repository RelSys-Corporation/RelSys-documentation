
Objetivo: Iniciar o fluxo de caixa bipando produtos. Requisitos Relacionados: [RF06, RF12] Pré-requisito: Sessão de caixa aberta (UC08).

Fluxo Principal:

1. O usuário acessa "Vendas" (PDV) [RF12].
    
2. O usuário bipa o código de barras com o leitor ou digita o nome [RF06].
    
3. O sistema lista os resultados e o usuário seleciona o item.
    
4. O item vai para o "Carrinho" e totaliza os valores.
    

Fluxos Alternativos:

- FA01 - Carrinho Vazio: Ao abrir a tela, o sistema exibe o ícone de carrinho e a mensagem (MSG22).
    

Mensagens do Sistema (UC09):

- MSG22: "Carrinho vazio. Adicione produtos por código de barras ou nome."




```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor User as Operador
    participant UI as Interface (PDV)
    participant API as API Backend
    participant DB as Banco de Dados

    Note over User,UI: Pré-requisito: Sessão de caixa aberta (UC08)
    User->>UI: Acessa "Vendas" (PDV) [RF12]
    UI-->>User: Exibe ícone de carrinho vazio e MSG22 (FA01)

    loop Adição de itens ao carrinho
        User->>UI: Bipa código de barras ou digita nome do produto [RF06]
        UI->>API: GET /api/v1/products/search?q={codigo_ou_nome}
        API->>DB: Busca produto ativo e saldo disponível
        DB-->>API: Retorna correspondências
        API-->>UI: Lista resultados
        User->>UI: Seleciona o item
        UI-->>User: Adiciona item ao "Carrinho" e totaliza valores
    end
```