
UC01 - Parametrizar Métodos de Pagamento Objetivo: Cadastrar e gerenciar as formas de pagamento aceitas. Requisitos Relacionados: [RF04] 

Fluxo Principal (Cadastrar Novo Método):

1. O administrador acessa o menu lateral e clica em "Métodos de Pagamento" [RF04].
    
2. O sistema exibe o painel de cadastro à esquerda e a lista de métodos já cadastrados à direita.
    
3. O administrador preenche o campo de texto "Nome do Método" (ex: "Cartão de Crédito - Visa/Master").
    
4. O administrador clica no botão azul "Cadastrar".
    
5. O sistema grava o dado no banco e exibe a mensagem (MSG01).
    
6. O sistema atualiza a tabela à direita, exibindo o novo método com uma chave (toggle switch) na coluna "Ativo" marcada como ligada por padrão.
    

Fluxos Alternativos:

- FA01 - Cadastro com Campo Vazio: No passo 4, o administrador clica em "Cadastrar" deixando o campo vazio. O sistema bloqueia a ação e exibe a mensagem (MSG02).
    
- FA02 - Ativar ou Desativar Método: O administrador clica na chave "Ativo" para desligar um método. O sistema atualiza o status no banco e exibe a mensagem (MSG03). Ao realizar o fluxo do PDV [RF14], este método inativado não aparecerá para o operador.
    

Mensagens do Sistema (UC01):

- MSG01: "Método de pagamento cadastrado com sucesso!"
    
- MSG02: "Informe o nome do método de pagamento."
    
- MSG03: "Status do método de pagamento atualizado."
    


```mermaid-next
sequenceDiagram
    autonumber
    actor Admin as Administrador
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    Admin->>UI: Acessa "Métodos de Pagamento"
    UI->>API: GET /api/v1/payment-methods
    API->>DB: Consultar métodos de pagamento
    DB-->>API: Retorna lista de métodos
    API-->>UI: Exibe painel de cadastro e lista à direita

    alt Fluxo Principal: Cadastrar Novo Método
        Admin->>UI: Preenche "Nome do Método" e clica em "Cadastrar"
        UI->>API: POST /api/v1/payment-methods (Nome)
        API->>DB: Salva novo método (Ativo=true)
        DB-->>API: Confirma gravação
        API-->>UI: Retorna Sucesso (MSG01)
        UI-->>Admin: Atualiza tabela com novo método ativo e exibe MSG01
    else FA01: Cadastro com Campo Vazio
        Admin->>UI: Clica em "Cadastrar" (Campo vazio)
        UI-->>Admin: Bloqueia ação e exibe MSG02
    else FA02: Ativar ou Desativar Método
        Admin->>UI: Clica na chave toggle "Ativo"
        UI->>API: PATCH /api/v1/payment-methods/{id}/status
        API->>DB: Atualiza status do método
        DB-->>API: Confirma alteração
        API-->>UI: Retorna Sucesso (MSG03)
        UI-->>Admin: Atualiza estado da chave e exibe MSG03
    end
```


