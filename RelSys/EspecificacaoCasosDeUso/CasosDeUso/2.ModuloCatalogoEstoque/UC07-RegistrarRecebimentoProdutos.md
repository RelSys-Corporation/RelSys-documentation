
Especificação do Caso de Uso: UC07
- Registrar Recebimento de Produtos Fluxo Principal (Registrar Recebimento com Sucesso):

1. O usuário acessa o menu lateral, expande a seção "Produtos" e clica em "Recebimento".
    
2. O sistema exibe a página dividida em dois painéis: "Registrar Recebimento" (à esquerda) e "Recebimentos Recentes" (à direita).
    
3. O usuário seleciona o item desejado no campo de lista suspensa "Produto".
    
4. O usuário informa o valor de custo no campo "R$ Preço de compra (unitário)".
    
5. O usuário informa o volume recebido no campo "Quantidade".
    
6. O usuário clica no botão azul "Registrar Recebimento".
    
7. O sistema valida se todos os dados foram inseridos corretamente.
    
8. O sistema processa a entrada no estoque (registrando como um novo lote para controle PEPS) e exibe uma mensagem de sucesso.
    
9. O sistema atualiza o painel direito ("Recebimentos Recentes"), listando o item recém-adicionado e substituindo a mensagem de estado vazio.
    

Fluxos Alternativos:

- FA01 - Campos Obrigatórios Vazios: No passo 6 do fluxo principal, o usuário clica em "Registrar Recebimento" sem preencher os campos. O sistema bloqueia a gravação e exibe a mensagem de erro: "Preencha todos os campos!".
    
- FA02 - Estado Inicial Sem Registros: Ao acessar a tela de Recebimento pela primeira vez na sessão atual, o painel "Recebimentos Recentes" exibe um ícone de histórico com a mensagem: "Nenhum recebimento registrado nesta sessão".
    

**