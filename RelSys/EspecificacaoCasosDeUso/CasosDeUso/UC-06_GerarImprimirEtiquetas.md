**

## Especificação do Caso de Uso: Gerar e Imprimir Etiquetas

Fluxo Principal (Adicionar Produto à Fila e Imprimir):

1. O usuário acessa o menu lateral e clica em "Etiquetas".
    
2. O sistema exibe a interface dividida entre o painel "Adicionar à Impressão" à esquerda e o painel "Fila de Impressão" à direita.
    
3. O usuário seleciona o item desejado através do menu suspenso no campo "Produto".
    
4. O usuário informa o número desejado no campo "Quantidade de etiquetas", que exibe "1" por padrão.
    
5. O usuário clica no botão "+ Adicionar".
    
6. O sistema atualiza o painel "Fila de Impressão", adicionando o item à lista (ex: "BatataTeste4" com a indicação de "1 etiqueta").
    
7. O sistema atualiza o contador no topo do painel direito (ex: "1 produto — 1 etiquetas").
    
8. O sistema disponibiliza o botão azul "Imprimir Etiquetas" na parte inferior do painel.
    
9. O usuário clica no botão "Imprimir Etiquetas" para iniciar a impressão.
    

Fluxos Alternativos:

- FA01 - Fila de Impressão Vazia (Estado Inicial):
    

1. Ao acessar a tela sem ter adicionado nenhum item, o painel da fila exibe a contagem "0 produtos — 0 etiquetas".
    
2. O sistema mostra um ícone de etiqueta seguido da mensagem principal "Nenhum produto na fila".
    
3. Abaixo, o sistema exibe a mensagem instrutiva "Adicione produtos para gerar etiquetas".
    

- FA02 - Remover Item da Fila:
    

1. Com um produto já presente na "Fila de Impressão", o usuário decide retirá-lo da lista.
    
2. O usuário clica no botão com o ícone de "X", localizado à direita do nome do produto.
    
3. O sistema remove o produto selecionado da interface e atualiza os contadores da fila de impressão.
    



**