**

## Especificação do Caso de Uso: UC01 - Realizar Venda (Fase: Adicionar Produto)

Fluxo Principal (Buscar e Adicionar Produto):

1. O usuário acessa o menu lateral e clica em "Vendas"venda, dividida em dois painéis: "Adicionar Produto" (esquerda) e "Carrinho" (direita).
    
2. O usuário clica no campo de busca que exibe o texto placeholder "Nome, ID ou código de barras".
    
3. O usuário digita o nome do produto, o ID, ou utiliza um leitor para bipar o código de barras (atendendo ao RF06).
    
4. O sistema exibe uma lista suspensa (dropdown) abaixo do campo de busca com os resultados correspondentes, mostrando o Nome, o ID/Código e o Preço do produto 
    
5. O usuário clica no produto desejado na lista.
    
6. O sistema transfere o item selecionado para o painel "Carrinho" à direita, atualizando a contagem de itens.
    

Fluxos Alternativos:

- FA01 - Carrinho Vazio (Estado Inicial):
    

1. Ao abrir a tela de Vendas ou após finalizar/cancelar uma venda anterior, o sistema exibe o painel direito com o ícone de um carrinho azul.
    
2. O sistema exibe os textos: "Carrinho vazio" e "Adicione produtos por código de barras ou nome", mantendo o contador em "0 itens".
    



**