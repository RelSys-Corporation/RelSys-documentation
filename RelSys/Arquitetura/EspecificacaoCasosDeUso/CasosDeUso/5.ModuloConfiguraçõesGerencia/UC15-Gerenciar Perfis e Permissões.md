
Objetivo: Criar perfis de acesso (cargos) e parametrizar suas permissões por módulo e funcionalidade, aplicando o Princípio do Menor Privilégio.

Requisitos Relacionados: [RF01], [RN05], [RNF04, RNF05]

Fluxo Principal (Criar Perfil e Conceder Permissões):

1. O administrador acessa o menu lateral e clica em "Configurações" > "Perfis e Permissões" [RF01].
    
2. O sistema exibe a lista de perfis cadastrados na empresa/filial atual [RNF05] e o administrador clica em "Novo Perfil".
    
3. O administrador informa o nome do perfil (ex: "Supervisor de Loja") e clica em "Salvar".
    
4. O sistema cria o perfil sem nenhuma permissão marcada por padrão, aplicando o Princípio do Menor Privilégio [RN05], e exibe a matriz de permissões disponíveis por módulo.
    
5. O administrador seleciona as permissões desejadas (ex: "Cancelar Venda", "Registrar Sangria", "Visualizar Dashboard") e clica em "Salvar Permissões".
    
6. O sistema grava os vínculos no banco de dados e exibe a mensagem (MSG31).
    

Fluxos Alternativos:

- FA01 - Nome de Perfil Vazio ou Duplicado: No passo 3, caso o nome do perfil esteja em branco ou já exista na filial, o sistema bloqueia o cadastro e exibe a mensagem (MSG32).
    
- FA02 - Editar Permissões de Perfil Existente: O administrador seleciona um perfil já cadastrado, marca ou desmarca permissões na matriz e clica em "Salvar Permissões". O sistema atualiza as regras de acesso para as próximas requisições [RNF04] e exibe a mensagem (MSG31).
    

Mensagens do Sistema (UC15):

MSG31: "Permissões do perfil salvas com sucesso!"

MSG32: "Informe um nome de perfil válido e não duplicado."

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor Admin as Administrador
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    Admin->>UI: Acessa "Configurações" > "Perfis e Permissões" [RF01]
    UI->>API: GET /api/v1/roles
    API->>DB: Consulta perfis cadastrados na filial atual [RNF05]
    DB-->>API: Retorna lista de perfis
    API-->>UI: Exibe listagem de perfis

    alt Fluxo Principal: Criar Perfil e Conceder Permissões
        Admin->>UI: Clica em "Novo Perfil", informa o nome (ex: "Supervisor de Loja") e clica em "Salvar"
        UI->>API: POST /api/v1/roles (Nome)
        API->>DB: Valida unicidade e cria perfil sem permissões por padrão [RN05]
        DB-->>API: Retorna perfil criado e matriz de permissões disponíveis
        API-->>UI: Exibe matriz de permissões por módulo (todas desmarcadas)
        Admin->>UI: Seleciona permissões desejadas e clica em "Salvar Permissões"
        UI->>API: PUT /api/v1/roles/{id}/permissions (ListaPermissoes)
        API->>DB: Grava vínculos de permissões do perfil
        DB-->>API: Confirma gravação
        API-->>UI: Sucesso (MSG31)
        UI-->>Admin: Exibe MSG31
    else FA01: Nome de Perfil Vazio ou Duplicado
        Admin->>UI: Submete nome em branco ou já existente na filial
        UI->>API: POST /api/v1/roles (Nome)
        API->>DB: Verifica unicidade na filial
        DB-->>API: Nome inválido ou duplicado
        API-->>UI: Erro de validação (MSG32)
        UI-->>Admin: Bloqueia cadastro e exibe MSG32
    else FA02: Editar Permissões de Perfil Existente
        Admin->>UI: Seleciona perfil existente, marca/desmarca permissões e clica em "Salvar Permissões"
        UI->>API: PUT /api/v1/roles/{id}/permissions (ListaPermissoes)
        API->>DB: Atualiza regras de acesso para próximas requisições [RNF04]
        DB-->>API: Confirma atualização
        API-->>UI: Sucesso (MSG31)
        UI-->>Admin: Exibe MSG31
    end
```