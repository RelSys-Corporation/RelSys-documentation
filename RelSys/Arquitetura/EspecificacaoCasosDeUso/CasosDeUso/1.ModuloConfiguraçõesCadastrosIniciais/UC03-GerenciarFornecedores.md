
Objetivo: Cadastrar fornecedores e configurar regras fiscais/comerciais (isenção). Requisitos Relacionados: [RF03]

Fluxo Principal (Cadastrar Novo Fornecedor):

1. O usuário acessa o menu lateral e clica em "Fornecedores" [RF03].
    
2. O sistema exibe o painel "Novo Fornecedor" e a lista de "Fornecedores Cadastrados".
    
3. O usuário preenche o campo de texto "Nome do fornecedor" e clica em "Cadastrar".
    
4. O sistema valida os dados e exibe a mensagem (MSG07).
    
5. O sistema atualiza a tabela, exibindo o novo fornecedor com a chave (toggle switch) na coluna ISENTO.
    

Fluxos Alternativos:

- FA01 - Cadastro com Campo Vazio: O usuário clica em "Cadastrar" sem preencher o nome. O sistema bloqueia a ação e exibe a mensagem (MSG08).
    
- FA02 - Cancelar Ação: O usuário clica em "Cancelar". O sistema aborta a operação e limpa os campos.
    
- FA03 - Editar Fornecedor: O usuário aciona a edição, altera o nome e clica em "Atualizar".
    
- FA04 - Gerenciar Isenção: O usuário interage com o toggle "ISENTO" para marcar/desmarcar a isenção de descontos para produtos deste fornecedor.
    

Mensagens do Sistema (UC03):

- MSG07: "Fornecedor cadastrado com sucesso!"
    
- MSG08: "Informe o nome do fornecedor."
    

```mermaid-next
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Visão (Interface)
    participant Controller as Controlador (Fornecedores)
    participant DB as Banco de Dados

    User->>UI: Acessa menu "Fornecedores" [RF03]
    UI->>Controller: buscarFornecedores()
    Controller->>DB: SELECT * FROM fornecedores
    DB-->>Controller: listaFornecedores
    Controller-->>UI: renderizaPainelELista(listaFornecedores)

    alt Fluxo Principal: Cadastrar Novo Fornecedor
        User->>UI: Preenche "Nome do fornecedor" e clica em "Cadastrar"
        UI->>Controller: cadastrarFornecedor(nome)
        Controller->>DB: INSERT INTO fornecedores (nome, isento) VALUES (nome, false)
        DB-->>Controller: sucesso
        Controller-->>UI: MSG07 ("Fornecedor cadastrado com sucesso!") e listaAtualizada
        UI-->>User: Exibe MSG07 e atualiza tabela com toggle ISENTO

    else FA01 - Cadastro com Campo Vazio
        User->>UI: Clica em "Cadastrar" sem nome
        UI->>UI: validaCampo() [Vazio]
        UI-->>User: Exibe MSG08 ("Informe o nome do fornecedor.") e bloqueia ação

    else FA02 - Cancelar Ação
        User->>UI: Clica em "Cancelar"
        UI->>UI: limparCampos()
        UI-->>User: Operação abortada e campos limpos

    else FA03 - Editar Fornecedor
        User->>UI: Altera nome do fornecedor e clica em "Atualizar"
        UI->>Controller: atualizarFornecedor(id, novoNome)
        Controller->>DB: UPDATE fornecedores SET nome = novoNome WHERE id = id
        DB-->>Controller: sucesso
        Controller-->>UI: Atualiza tabela

    else FA04 - Gerenciar Isenção
        User->>UI: Interage com toggle "ISENTO"
        UI->>Controller: alterarIsencao(id, statusIsento)
        Controller->>DB: UPDATE fornecedores SET isento = statusIsento WHERE id = id
        DB-->>Controller: sucesso
        Controller-->>UI: Atualiza estado do toggle na tela
    end
```