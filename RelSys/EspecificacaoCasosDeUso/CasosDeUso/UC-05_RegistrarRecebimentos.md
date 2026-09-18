**

## Especificação do Caso de Uso: UC05 - Registrar Recebimento de Produtos

  

Fluxo Principal (Registrar Recebimento com Sucesso):

1. O usuário acessa o menu lateral, expande a seção "Produtos" e clica em "Recebimento".
    
2. O sistema exibe a página dividida em dois painéis: "Registrar Recebimento" (à esquerda) e "Recebimentos Recentes" (à direita).
    
3. O usuário seleciona o item desejado no campo de lista suspensa "Produto".
    
4. O usuário informa o valor de custo no campo "R$ Preço de compra (unitário)".
    
5. O usuário informa o volume recebido no campo "Quantidade".
    
6. O usuário clica no botão azul "Registrar Recebimento".
    
7. O sistema valida se todos os dados foram inseridos corretamente.
    
8. O sistema processa a entrada no estoque e exibe uma mensagem de sucesso.
    
9. O sistema atualiza o painel direito ("Recebimentos Recentes"), listando o item recém-adicionado e substituindo a mensagem de estado vazio.
    

Fluxos Alternativos:

- FA01 - Campos Obrigatórios Vazios:
    

1. No passo 6 do fluxo principal, o usuário clica no botão "Registrar Recebimento" sem ter preenchido um ou mais campos do formulário (Produto, Preço de compra ou Quantidade).
    
2. O sistema bloqueia a gravação e exibe a mensagem de erro: "Preencha todos os campos!".
    
3. O sistema aguarda o usuário corrigir as informações antes de tentar novamente.
    

- FA02 - Estado Inicial Sem Registros:
    

1. Ao acessar a tela de Recebimento pela primeira vez na sessão atual (ou antes de qualquer registro ser feito), o painel "Recebimentos Recentes" à direita não exibe nenhuma lista.
    
2. Em vez disso, o sistema exibe um ícone de histórico com a mensagem: "Nenhum recebimento registrado nesta sessão".
    



**