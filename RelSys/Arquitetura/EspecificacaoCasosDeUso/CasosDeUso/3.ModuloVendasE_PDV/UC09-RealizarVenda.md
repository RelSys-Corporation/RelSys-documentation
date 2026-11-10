
UC09 - Realizar Venda (Adicionar Produto no PDV) Objetivo: Iniciar o fluxo de caixa bipando produtos. Requisitos Relacionados: [RF06, RF12] Pré-requisito: Sessão de caixa aberta (UC08). Fluxo Principal:

1. O usuário acessa "Vendas" (PDV) [RF12].
    
2. O usuário bipa o código de barras com o leitor ou digita o nome [RF06].
    
3. O sistema lista os resultados, o usuário seleciona.
    
4. O item vai para o "Carrinho" e totaliza os valores.
    

Fluxos Alternativos:

- FA01 - Carrinho Vazio: Ao abrir a tela, o sistema exibe o ícone de carrinho e a mensagem (MSG19).
    

Mensagens do Sistema (UC09):
- MSG19: "Carrinho vazio. Adicione produtos por código de barras ou nome."



```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor Operator as Operador
    participant UI as Interface (PDV)
    participant API as API Backend
    participant DB as Banco de Dados

    Operator->>UI: Acessa "Vendas" (PDV) [RF12]
    note over UI: Pré-requisito: Sessão de caixa aberta (UC08)

    alt FA01: Carrinho Vazio na Abertura da Tela
        UI-->>Operator: Exibe ícone do carrinho e MSG19
    end

    Operator->>UI: Bipa código de barras com leitor ou digita nome [RF06]
    UI->>API: GET /api/v1/products/search?q={codigo_ou_nome}
    API->>DB: Busca produto ativo por Código de Barras ou Nome
    DB-->>API: Retorna produto encontrado
    API-->>UI: Exibe resultados da busca
    Operator->>UI: Seleciona o produto desejado
    UI-->>UI: Adiciona produto ao Carrinho e recalcula totalizadores
    UI-->>Operator: Atualiza visualização do carrinho com itens e totais
```