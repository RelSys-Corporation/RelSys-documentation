**

## Especificação do Caso de Uso: UC02 - Cadastro de Produto

Fluxo Principal (Cadastrar Produto com Sucesso):

1. O usuário acessa o menu lateral, expande a seção "Produtos" e clica em "Cadastro"
    
2. O sistema exibe o painel "Novo Produto" com os campos de preenchimento.
    
3. O usuário preenche o "Nome do produto".
    
4. O usuário digita o "Código de barras" (ou deixa em branco para geração automática).
    
5. O usuário seleciona um "Fornecedor" na lista suspensa (opcional).
    
6. O usuário preenche o campo "R$ Preço de venda" utilizando apenas números.
    
7. O usuário clica no botão azul "Cadastrar Produto".
    
8. O sistema grava o produto e exibe a mensagem de sucesso: "Produto cadastrado com sucesso!".
    

Fluxos Alternativos:

- FA01 - Campos Obrigatórios Vazios:
    

1. No passo 7 do fluxo principal, o usuário tenta cadastrar deixando campos obrigatórios vazios (como Nome do produto ou R$ Preço de venda). O campo Fornecedor e o Código de barras estão isentos dessa obrigatoriedade.
    
2. O sistema bloqueia a ação e exibe a mensagem: "preencha todos os campos".
    
3. O sistema aguarda a correção pelo usuário.
    

- FA02 - Formato de Preço Inválido:
    

1. No passo 6 do fluxo principal, o usuário digita alguma letra no campo "R$ Preço de venda".
    
2. Ao clicar em cadastrar, o sistema detecta a inconsistência no tipo de dado.
    
3. O sistema bloqueia o cadastro e exibe a mensagem: "Preencha todos os campos corretamente".
    
4. O sistema aguarda o usuário inserir apenas valores numéricos válidos.
    

- FA03 - Navegar para Listagem:
    

1. A qualquer momento na tela de cadastro, o usuário clica no botão branco "Ver Produtos".
    
2. O sistema interrompe o cadastro atual e redireciona o usuário para a tela de listagem do estoque.
**