**

Especificação do Caso de Uso: UC01 - Parametrizar Métodos de Pagamento Fluxo Principal (Cadastrar Novo Método):

1. O administrador acessa o menu lateral e clica em "Métodos de Pagamento".
    
2. O sistema exibe o painel de cadastro à esquerda e a lista de métodos já cadastrados à direita.
    
3. O administrador preenche o campo de texto "Nome do Método" (ex: "Cartão de Crédito - Visa/Master").
    
4. O administrador clica no botão azul "Cadastrar".
    
5. O sistema grava o dado no banco e exibe a mensagem: "Método de pagamento cadastrado com sucesso!".
    
6. O sistema atualiza a tabela à direita, exibindo o novo método com uma chave (toggle switch) na coluna "Ativo" marcada como ligada por padrão.
    

Fluxos Alternativos:

- FA01 - Cadastro com Campo Vazio: No passo 4 do fluxo principal, o administrador clica em "Cadastrar" deixando o campo de nome vazio. O sistema bloqueia a ação e exibe a mensagem: "Informe o nome do método de pagamento".
    
- FA02 - Ativar ou Desativar Método (Inativação Lógica): O administrador visualiza a lista de métodos cadastrados à direita. O administrador clica na chave (toggle switch) na coluna "Ativo" para desligar um método (ex: o estabelecimento parou de aceitar cheque). O sistema atualiza o status no banco de dados e exibe a mensagem: "Status do método de pagamento atualizado". Ao realizar o fluxo do PDV (Vendas), este método inativado não aparecerá na lista de opções para o operador de caixa selecionar.
    

**