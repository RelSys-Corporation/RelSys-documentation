
Objetivo: Cadastrar fornecedores e configurar regras fiscais/comerciais (isenção de desconto). Requisitos Relacionados: [RF03], [RN01] 

Fluxo Principal (Cadastrar Novo Fornecedor):

1. O usuário acessa o menu lateral e clica em "Fornecedores" [RF03].
    
2. O sistema exibe o painel "Novo Fornecedor" e a lista de "Fornecedores Cadastrados".
    
3. O usuário preenche o campo de texto "Nome do fornecedor" e clica em "Cadastrar".
    
4. O sistema valida os dados e exibe a mensagem (MSG07).
    
5. O sistema atualiza a tabela, exibindo o novo fornecedor com a chave (toggle switch) na coluna "ISENTO".
    

Fluxos Alternativos:

- FA01 - Cadastro com Campo Vazio: O usuário clica em "Cadastrar" sem preencher o nome. O sistema bloqueia a ação e exibe a mensagem (MSG08).
    
- FA02 - Cancelar Ação: O usuário clica em "Cancelar". O sistema aborta a operação e limpa os campos.
    
- FA03 - Editar Fornecedor: O usuário aciona a edição, altera o nome e clica em "Atualizar".
    
- FA04 - Gerenciar Isenção: O usuário interage com o toggle "ISENTO" para marcar/desmarcar a isenção de descontos para produtos deste fornecedor [RN01].
    

Mensagens do Sistema (UC03):

- MSG07: "Fornecedor cadastrado com sucesso!"
    
- MSG08: "Informe o nome do fornecedor."
    

**
    
    

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    User->>UI: Acessa menu lateral > "Fornecedores" [RF03]
    UI->>API: GET /api/v1/suppliers
    API->>DB: Consulta fornecedores cadastrados
    DB-->>API: Retorna lista
    API-->>UI: Exibe painel "Novo Fornecedor" e tabela

    alt Fluxo Principal: Cadastrar Novo Fornecedor
        User->>UI: Preenche "Nome do fornecedor" e clica em "Cadastrar"
        UI->>API: POST /api/v1/suppliers (Nome)
        API->>DB: Valida e grava fornecedor
        DB-->>API: Confirma gravação
        API-->>UI: Sucesso (MSG07)
        UI-->>User: Atualiza tabela com coluna "ISENTO" e exibe MSG07
    else FA01: Cadastro com Campo Vazio
        User->>UI: Clica em "Cadastrar" sem preencher o nome
        UI-->>User: Bloqueia ação e exibe MSG08
    else FA02: Cancelar Ação
        User->>UI: Clica em "Cancelar"
        UI-->>User: Aborta operação e limpa os campos
    else FA03: Editar Fornecedor
        User->>UI: Aciona edição, altera nome e clica em "Atualizar"
        UI->>API: PUT /api/v1/suppliers/{id} (Nome)
        API->>DB: Atualiza registro
        DB-->>API: Confirma alteração
        API-->>UI: Atualiza tabela
    else FA04: Gerenciar Isenção
        User->>UI: Interage com o toggle "ISENTO" [RN01]
        UI->>API: PATCH /api/v1/suppliers/{id}/exemption
        API->>DB: Atualiza regra de isenção de desconto
        DB-->>API: Confirma alteração
        API-->>UI: Atualiza status do toggle na tabela
    end
```