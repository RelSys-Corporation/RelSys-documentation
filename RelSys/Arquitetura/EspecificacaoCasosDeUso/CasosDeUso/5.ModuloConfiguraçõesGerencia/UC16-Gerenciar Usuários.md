
Objetivo: Cadastrar colaboradores no sistema, vincular seus respectivos perfis de acesso e gerenciar o status de ativação de suas contas.

Requisitos Relacionados: [RF01], [RN04, RN05], [RNF03, RNF04, RNF05]

Pré-requisito: Existência de pelo menos um perfil de acesso previamente cadastrado (UC15).

Fluxo Principal (Cadastrar Novo Usuário):

1. O administrador acessa o menu lateral e clica em "Configurações" > "Usuários" [RF01].
    
2. O sistema exibe a listagem de usuários da empresa/filial atual [RNF05] e o administrador clica em "Novo Usuário".
    
3. O administrador preenche nome completo, e-mail de acesso, senha inicial e seleciona o "Perfil de Acesso" (cadastrado no UC15) e a filial de vínculo [RNF05].
    
4. O administrador clica em "Salvar".
    
5. O sistema valida os dados, armazena a senha criptografada (hash seguro) [RNF03], registra o usuário como ativo e exibe a mensagem (MSG33).
    

Fluxos Alternativos:

- FA01 - Dados Obrigatórios Vazios ou E-mail Duplicado: No passo 4, caso algum campo obrigatório esteja em branco ou o e-mail já esteja cadastrado, o sistema bloqueia a gravação e exibe a mensagem (MSG34).
    
- FA02 - Editar Usuário ou Alterar Perfil: O administrador abre o cadastro de um usuário existente, altera seus dados ou troca seu perfil de acesso e clica em "Salvar". O sistema atualiza o registro e exibe a mensagem (MSG35).
    
- FA03 - Ativar ou Inativar Usuário: O administrador desativa a chave "Ativo" de um colaborador desligado. O sistema bloqueia imediatamente novos logins desse usuário [RNF04], preservando seu registro histórico nas vendas e movimentações de caixa já realizadas [RN04], e exibe a mensagem (MSG36).
    

Mensagens do Sistema (UC16):

MSG33: "Usuário cadastrado com sucesso!"

MSG34: "Preencha todos os campos obrigatórios ou informe um e-mail não cadastrado."

MSG35: "Dados do usuário atualizados com sucesso!"

MSG36: "Status do usuário atualizado com sucesso."

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor Admin as Administrador
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    Note over Admin,UI: Pré-requisito: Perfil de acesso previamente cadastrado (UC15)
    Admin->>UI: Acessa "Configurações" > "Usuários" [RF01]
    UI->>API: GET /api/v1/users
    API->>DB: Consulta usuários da empresa/filial atual [RNF05]
    DB-->>API: Retorna listagem de usuários
    API-->>UI: Exibe lista de usuários

    alt Fluxo Principal: Cadastrar Novo Usuário
        Admin->>UI: Clica em "Novo Usuário", preenche nome, e-mail, senha, perfil (UC15) e filial [RNF05]
        Admin->>UI: Clica em "Salvar"
        UI->>API: POST /api/v1/users (DadosUsuario)
        API->>API: Valida campos e gera hash seguro da senha [RNF03]
        API->>DB: Verifica duplicidade de e-mail e grava usuário (Ativo=true)
        DB-->>API: Confirma gravação
        API-->>UI: Sucesso (MSG33)
        UI-->>Admin: Exibe MSG33 e atualiza listagem
    else FA01: Dados Obrigatórios Vazios ou E-mail Duplicado
        Admin->>UI: Submete formulário incompleto ou com e-mail já cadastrado
        UI->>API: POST /api/v1/users (DadosUsuario)
        API->>DB: Verifica existência do e-mail
        DB-->>API: E-mail duplicado ou campos inválidos
        API-->>UI: Erro de validação (MSG34)
        UI-->>Admin: Bloqueia gravação e exibe MSG34
    else FA02: Editar Usuário ou Alterar Perfil
        Admin->>UI: Abre usuário existente, altera dados ou perfil de acesso e clica em "Salvar"
        UI->>API: PUT /api/v1/users/{id} (DadosAtualizados)
        API->>DB: Atualiza cadastro e perfil vinculado
        DB-->>API: Confirma atualização
        API-->>UI: Sucesso (MSG35)
        UI-->>Admin: Exibe MSG35
    else FA03: Ativar ou Inativar Usuário
        Admin->>UI: Desativa chave "Ativo" de colaborador desligado
        UI->>API: PATCH /api/v1/users/{id}/status (Ativo=false)
        API->>DB: Inativa conta bloqueando novos logins [RNF04] e preserva histórico [RN04]
        DB-->>API: Confirma alteração de status
        API-->>UI: Sucesso (MSG36)
        UI-->>Admin: Exibe MSG36
    end
```