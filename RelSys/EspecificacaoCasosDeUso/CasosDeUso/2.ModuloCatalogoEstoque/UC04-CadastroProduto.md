
Especificação do Caso de Uso: UC04 - Cadastro de Produto Fluxo Principal (Cadastrar Produto com Sucesso):

1. O usuário acessa o menu lateral, expande a seção "Produtos" e clica em "Cadastro".
    
2. O sistema exibe o painel "Novo Produto" com os campos de preenchimento.
    
3. O usuário preenche o "Nome do produto".
    
4. O usuário digita o "Código de barras" (ou deixa em branco para geração automática).
    
5. O usuário seleciona um "Fornecedor" na lista suspensa (opcional).
    
6. O usuário interage com a chave (toggle switch) "Isenção de Desconto" para ativar ou desativar essa restrição para o produto/fornecedor (atendendo ao RF05 e RN01).
    
7. O usuário preenche o campo "R$ Preço de venda" utilizando apenas números.
    
8. O usuário clica no botão azul "Cadastrar Produto".
    
9. O sistema gera automaticamente um código de barras interno único (RN03) caso o campo correspondente tenha sido deixado em branco no passo 4 (atendendo ao RF07).
    
10. O sistema grava o produto e exibe a mensagem de sucesso: "Produto cadastrado com sucesso!".
    

Fluxos Alternativos:

- FA01 - Campos Obrigatórios Vazios: No passo 8 do fluxo principal, o usuário tenta cadastrar deixando campos obrigatórios vazios (como Nome do produto ou R$ Preço de venda). O campo Fornecedor e o Código de barras estão isentos dessa obrigatoriedade. O sistema bloqueia a ação, exibe a mensagem: "preencha todos os campos" e aguarda a correção.
    
- FA02 - Formato de Preço Inválido: No passo 7 do fluxo principal, o usuário digita alguma letra no campo "R$ Preço de venda". Ao clicar em cadastrar, o sistema detecta a inconsistência, bloqueia o cadastro, exibe a mensagem "Preencha todos os campos corretamente" e aguarda valores numéricos.
    
- FA03 - Navegar para Listagem: A qualquer momento na tela de cadastro, o usuário clica no botão branco "Ver Produtos". O sistema interrompe o cadastro atual e redireciona o usuário para a tela de listagem do estoque.
    

**