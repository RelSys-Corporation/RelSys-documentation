**

Especificação do Caso de Uso: UC03 - Gerenciar Fornecedores Fluxo Principal (Cadastrar Novo Fornecedor):

1. O usuário acessa o menu lateral e clica em "Fornecedores".
    
2. O sistema exibe a interface com o painel "Novo Fornecedor" à esquerda e a lista de "Fornecedores Cadastrados" à direita.
    
3. O usuário preenche o campo de texto "Nome do fornecedor".
    
4. O usuário clica no botão azul "Cadastrar".
    
5. O sistema valida os dados e exibe a mensagem de sucesso: "Fornecedor cadastrado com sucesso!".
    
6. O sistema atualiza a tabela à direita, exibindo o novo fornecedor com seu respectivo ID, NOME e a chave (toggle switch) na coluna ISENTO.
    

Fluxos Alternativos:

- FA01 - Cadastro com Campo Vazio: No passo 4 do fluxo principal, o usuário clica em "Cadastrar" deixando o campo "Nome do fornecedor" vazio. O sistema bloqueia a ação e exibe a mensagem: "Informe o nome do fornecedor" e aguarda o preenchimento.
    
- FA02 - Cancelar Ação (Cadastro ou Edição): Enquanto visualiza o painel "Novo Fornecedor" ou "Editar Fornecedor", o usuário clica no botão branco "Cancelar". O sistema cancela a operação atual e limpa o campo de texto.
    
- FA03 - Editar Fornecedor (Planejado / Em Desenvolvimento): O usuário aciona a edição de um fornecedor já existente. O painel esquerdo muda para "Editar Fornecedor", carregando o nome atual do fornecedor no campo de texto. O usuário altera o texto no campo "Nome do fornecedor" e clica no botão azul "Atualizar", que permite alterar o nome ou excluir o fornecedor.
    
- FA04 - Gerenciar Isenção: O usuário visualiza a lista de "Fornecedores Cadastrados" à direita. O usuário interage com o botão do tipo interruptor (toggle) na coluna "ISENTO" para marcar ou desmarcar a isenção de descontos para produtos vinculados a esse fornecedor específico.
    

**