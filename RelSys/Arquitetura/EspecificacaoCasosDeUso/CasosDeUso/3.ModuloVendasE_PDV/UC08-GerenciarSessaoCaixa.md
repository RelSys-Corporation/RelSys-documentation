UC08 - Gerenciar Sessão de Caixa Objetivo: Controlar abertura, reforço, sangria e fechamento do caixa operacional. Requisitos Relacionados: [RF12] Fluxo Principal (Abertura de Caixa):

1. O Usuário acessa o menu lateral e clica em "Vendas" (PDV) [RF12].
    
2. O sistema detecta que não há um caixa aberto para o usuário e exibe o modal de "Abertura de Caixa".
    
3. O Usuário informa o "Saldo Inicial (Fundo de Troco)" e clica em "Abrir Caixa".
    
4. O sistema registra a sessão no módulo financeiro (CashProcess) e exibe a mensagem (MSG17).
    
5. O sistema redireciona o usuário para a interface de vendas do PDV (UC09).
    

Fluxos Alternativos:

- FA01 - Fechamento de Caixa: O usuário clica no botão "Fechar Caixa" no PDV. O sistema consolida todos os pagamentos e movimentações, exibe o resumo cego, solicita a confirmação e encerra a sessão, exibindo a mensagem (MSG18).
    
Mensagens do Sistema (UC08): 

- MSG17: "Caixa aberto com sucesso. Boas vendas!" 

- MSG18: "Sessão de caixa encerrada."

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor Operator as Operador
    participant UI as Interface (PDV)
    participant API as API Backend (CashProcess)
    participant DB as Banco de Dados

    Operator->>UI: Acessa "Vendas" (PDV) [RF12]
    UI->>API: GET /api/v1/cash-sessions/active
    API->>DB: Verifica se há sessão de caixa aberta para o operador
    DB-->>API: Nenhuma sessão aberta encontrada

    alt Fluxo Principal: Abertura de Caixa
        API-->>UI: Retorna status "Caixa Fechado"
        UI-->>Operator: Exibe modal "Abertura de Caixa"
        Operator->>UI: Informa "Saldo Inicial (Fundo de Troco)" e clica em "Abrir Caixa"
        UI->>API: POST /api/v1/cash-sessions/open (FundoTroco)
        API->>DB: Cria nova sessão financeira (CashProcess)
        DB-->>API: Confirma abertura
        API-->>UI: Retorna Sucesso (MSG17)
        UI-->>Operator: Exibe MSG17 e redireciona para tela de Vendas do PDV (UC09)
    else FA01: Fechamento de Caixa
        Operator->>UI: Clica no botão "Fechar Caixa" no PDV
        UI->>API: GET /api/v1/cash-sessions/summary
        API->>DB: Consolida pagamentos e movimentações da sessão
        DB-->>API: Retorna totais financeiros
        API-->>UI: Exibe resumo cego e solicita confirmação
        Operator->>UI: Confirma fechamento do caixa
        UI->>API: POST /api/v1/cash-sessions/close
        API->>DB: Atualiza status da sessão para encerrado
        DB-->>API: Confirma encerramento
        API-->>UI: Retorna Sucesso (MSG18)
        UI-->>Operator: Exibe MSG18 e bloqueia PDV
    end
```