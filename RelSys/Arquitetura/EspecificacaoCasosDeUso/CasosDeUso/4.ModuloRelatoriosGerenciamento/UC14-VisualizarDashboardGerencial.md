
Objetivo: Prover visão rápida da saúde financeira e operacional da filial ao acessar o sistema. Requisitos Relacionados: [RF18], [RN05], [RNF05] 
Fluxo Principal:

1. O gerente/administrador faz login no sistema.
    
2. O sistema direciona o usuário para o Dashboard [RF18].
    
3. O sistema calcula em tempo real os widgets de: Faturamento do Dia, Produtos Mais Vendidos, Alertas de Estoque Baixo e Alertas de Lotes Próximos do Vencimento (exibindo produto, código do lote, saldo restante e dias para vencer), garantindo isolamento pelos dados da filial logada [RNF05].
    
4. O usuário interage com os gráficos e alertas.
    

Fluxos Alternativos:

- FA01 - Acesso sem Privilégios: Um Operador de Caixa faz login. Aplicando o Princípio do Menor Privilégio [RN05] e o controle de permissões por perfil de acesso, o sistema bloqueia os dados financeiros, oculta o Dashboard e redireciona o operador diretamente para o PDV (UC08).
    
    

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor User as Administrador
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    User->>UI: Faz login no sistema
    UI->>API: POST /api/v1/auth/login (Credenciais)
    API->>DB: Valida credenciais, filial e permissões do perfil [RN05]
    DB-->>API: Retorna sessão autenticada e permissões

    alt Fluxo Principal: Perfil Gerente / Administrador
        API-->>UI: Sessão autorizada para Dashboard
        UI->>API: GET /api/v1/dashboard/summary
        API->>DB: Calcula Faturamento do Dia, Top Produtos, Estoque Baixo e Lotes a Vencer (Filial) [RNF05]
        DB-->>API: Retorna indicadores em tempo real
        API-->>UI: Direciona e renderiza Dashboard [RF18]
        UI-->>User: Exibe gráficos, métricas e alertas de validade
    else FA01: Acesso sem Privilégios (Operador de Caixa)
        API-->>UI: Sessão restrita (Princípio do Menor Privilégio [RN05])
        UI-->>User: Bloqueia dados financeiros, oculta Dashboard e redireciona ao PDV (UC08)
    end
```