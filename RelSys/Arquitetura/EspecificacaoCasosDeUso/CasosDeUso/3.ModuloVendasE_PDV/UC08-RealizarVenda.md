
Objetivo: Iniciar o fluxo de caixa bipando produtos. Requisitos Relacionados: [RF06, RF12]

Fluxo Principal:

1. O usuário acessa "Vendas" (PDV) [RF12].
    
2. O usuário bipa o código de barras com o leitor ou digita o nome [RF06].
    
3. O sistema lista os resultados, o usuário seleciona.
    
4. O item vai para o "Carrinho" e totaliza os valores.
    

Fluxos Alternativos:

- FA01 - Carrinho Vazio: Ao abrir a tela, o sistema exibe o ícone de carrinho e a mensagem (MSG17).
    

Mensagens do Sistema (UC08):

- MSG17: "Carrinho vazio. Adicione produtos por código de barras ou nome."
    

```mermaid-next
sequenceDiagram
    autonumber
    actor User as Usuario
    participant UI as Visão (PDV)
    participant Controller as Controlador (Vendas/PDV)
    participant DB as Banco de Dados

    User->>UI: Acessa menu "Vendas" (PDV) [RF12]

    opt FA01 - Carrinho Vazio (Ao abrir)
        UI-->>User: Exibe ícone de carrinho e MSG17 ("Carrinho vazio. Adicione produtos...")
    end

    User->>UI: Bipa código de barras ou digita nome [RF06]
    UI->>Controller: buscarProdutoPDV(termo)
    Controller->>DB: SELECT * FROM produtos WHERE codigo_barras = termo OR nome LIKE %termo%
    DB-->>Controller: listaResultados
    Controller-->>UI: exibeResultados(listaResultados)

    User->>UI: Seleciona o produto desejado
    UI->>Controller: adicionarAoCarrinho(produtoId)
    Controller->>DB: SELECT * FROM produtos WHERE id = produtoId
    DB-->>Controller: dadosProduto
    Controller-->>UI: itemAdicionado(produto, valorUnitario)
    UI->>UI: recalcularTotalCarrinho()
    UI-->>User: Exibe item no Carrinho e totalizador atualizado
```