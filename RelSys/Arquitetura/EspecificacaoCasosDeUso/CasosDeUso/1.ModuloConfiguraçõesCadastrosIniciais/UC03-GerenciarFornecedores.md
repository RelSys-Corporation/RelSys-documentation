
UC03 - Gerenciar Fornecedores Objetivo: Cadastrar fornecedores e configurar regras fiscais/comerciais (isenção). Requisitos Relacionados: [RF03] Fluxo Principal (Cadastrar Novo Fornecedor):

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
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    User->>UI: Acessa "Fornecedores"
    UI->>API: GET /api/v1/suppliers
    API->>DB: Buscar fornecedores
    DB-->>API: Retorna lista
    API-->>UI: Exibe painel "Novo Fornecedor" e lista cadastrada

    alt Fluxo Principal: Cadastrar Novo Fornecedor
        User->>UI: Preenche "Nome do Fornecedor" e clica em "Cadastrar"
        UI->>API: POST /api/v1/suppliers (Nome)
        API->>DB: Persiste fornecedor
        DB-->>API: Confirma gravação
        API-->>UI: Retorna Sucesso (MSG07)
        UI-->>User: Atualiza tabela com coluna ISENTO e exibe MSG07
    else FA01: Cadastro com Campo Vazio
        User->>UI: Clica em "Cadastrar" sem preencher nome
        UI-->>User: Bloqueia ação e exibe MSG08
    else FA02: Cancelar Ação
        User->>UI: Clica em "Cancelar"
        UI-->>User: Aborta operação e limpa os campos
    else FA03: Editar Fornecedor
        User->>UI: Altera nome e clica em "Atualizar"
        UI->>API: PUT /api/v1/suppliers/{id}
        API->>DB: Atualiza registro
        DB-->>API: Confirmação
        API-->>UI: Atualiza lista
    else FA04: Gerenciar Isenção de Desconto
        User->>UI: Interage com o toggle "ISENTO"
        UI->>API: PATCH /api/v1/suppliers/{id}/exemption
        API->>DB: Atualiza flag de isenção
        DB-->>API: Confirmação
        API-->>UI: Atualiza status do toggle na UI
    end
```