**

## Especificação do Caso de Uso: UC04 - Consulta e Listagem de Produtos

Fluxo Principal (Visualizar e Buscar Produto):

1. O usuário acessa o menu lateral, expande a seção "Produtos" e clica em "Listagem".
    
2. O sistema carrega a página e exibe uma tabela contendo todos os itens cadastrados, divididos nas colunas: ID, Nome, Código de Barras e Preço.
    
3. O usuário clica no campo de texto "Buscar produto".
    
4. O usuário digita o nome, o ID ou o código de barras do item que deseja encontrar.
    
5. O sistema filtra a tabela automaticamente e exibe apenas os produtos que correspondem ao termo digitado.
    

Fluxos Alternativos:

- FA01 - Busca Sem Resultados:
    

1. No passo 4 do fluxo principal, o usuário digita um termo de busca que não existe no banco de dados.
    
2. O sistema oculta a tabela de produtos.
    
3. O sistema exibe o ícone de uma caixa vazia acompanhado da mensagem principal: "Nenhum produto encontrado".
    
4. O sistema exibe a mensagem secundária: "Tente alterar o termo de busca".
    
5. O sistema aguarda o usuário limpar ou alterar o texto no campo de busca para exibir a tabela novamente.
    

- FA02 - Ir para Cadastro de Novo Produto:
    

1. A qualquer momento na tela de listagem, o usuário clica no botão azul "+ Novo Produto".
    
2. O sistema sai da tela de listagem e redireciona o usuário para a tela de Cadastro de Produto
    



**