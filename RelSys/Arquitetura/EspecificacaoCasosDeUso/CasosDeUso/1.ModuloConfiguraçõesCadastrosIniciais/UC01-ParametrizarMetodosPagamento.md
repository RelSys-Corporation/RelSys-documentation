
Objetivo: Cadastrar e gerenciar as formas de pagamento aceitas. Requisitos Relacionados: [RF04], [RN04] 

Fluxo Principal (Cadastrar Novo Método):

1. O administrador acessa o menu lateral e clica em "Métodos de Pagamento" [RF04].
    
2. O sistema exibe o painel de cadastro à esquerda e a lista de métodos já cadastrados à direita.
    
3. O administrador preenche o campo de texto "Nome do Método" (ex: "Cartão de Crédito - Visa/Master").
    
4. O administrador clica no botão "Cadastrar".
    
5. O sistema grava o dado no banco vinculado à filial atual e exibe a mensagem (MSG01).
    
6. O sistema atualiza a tabela à direita, exibindo o novo método com uma chave (toggle switch) na coluna "Ativo" marcada como ligada por padrão.
    

Fluxos Alternativos:

- FA01 - Cadastro com Campo Vazio: No passo 4, o administrador clica em "Cadastrar" deixando o campo vazio. O sistema bloqueia a ação e exibe a mensagem (MSG02).
    
- FA02 - Ativar ou Desativar Método: O administrador clica na chave "Ativo" para desligar um método. O sistema atualiza o status no banco (sem excluir o registro, preservando o histórico [RN04]) e exibe a mensagem (MSG03). Ao realizar o fluxo do PDV [RF14], este método inativado não aparecerá para o operador.
    

Mensagens do Sistema (UC01):

- MSG01: "Método de pagamento cadastrado com sucesso!"
    
- MSG02: "Informe o nome do método de pagamento."
    
- MSG03: "Status do método de pagamento atualizado."
    

**
    


```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor Admin as Administrador
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    Admin->>UI: Acessa menu lateral > "Métodos de Pagamento" [RF04]
    UI->>API: GET /api/v1/payment-methods
    API->>DB: Consulta métodos da filial atual
    DB-->>API: Retorna lista de métodos
    API-->>UI: Exibe painel de cadastro e lista à direita

    alt Fluxo Principal: Cadastrar Novo Método
        Admin->>UI: Preenche "Nome do Método" e clica em "Cadastrar"
        UI->>API: POST /api/v1/payment-methods (Nome)
        API->>DB: Grava método vinculado à filial (Ativo=true)
        DB-->>API: Confirma gravação
        API-->>UI: Sucesso (MSG01)
        UI-->>Admin: Atualiza tabela (chave Ativo ligada) e exibe MSG01
    else FA01: Cadastro com Campo Vazio
        Admin->>UI: Clica em "Cadastrar" deixando campo vazio
        UI-->>Admin: Bloqueia ação e exibe MSG02
    else FA02: Ativar ou Desativar Método
        Admin->>UI: Clica na chave "Ativo"
        UI->>API: PATCH /api/v1/payment-methods/{id}/status
        API->>DB: Atualiza status (sem excluir, preserva histórico [RN04])
        DB-->>API: Confirma alteração
        API-->>UI: Sucesso (MSG03)
        UI-->>Admin: Atualiza chave na tabela e exibe MSG03
    end
```


