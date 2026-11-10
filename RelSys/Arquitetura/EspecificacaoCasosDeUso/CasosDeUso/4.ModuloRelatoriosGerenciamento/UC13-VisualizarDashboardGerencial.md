
UC13 - Visualizar Dashboard Gerencial Objetivo: Prover visão rápida da saúde do negócio ao acessar o sistema. Requisitos Relacionados: [RF16], [RN05] Fluxo Principal:

1. O Administrador faz login no sistema.
    
2. O sistema direciona o Administrador para o Dashboard [RF16].
    
3. O sistema calcula em tempo real os widgets de: Faturamento do Dia, Produtos Mais Vendidos e Alertas de Estoque Baixo, garantindo que os dados correspondam apenas à filial logada.
    
4. O Administrador interage com os gráficos.
    

Fluxos Alternativos:

- FA01 - Acesso sem Privilégios: Um Operador de Caixa faz login. Aplicando o Princípio do Menor Privilégio [RN05] e controle de permissões (Roles), o sistema bloqueia os dados financeiros, oculta o Dashboard e redireciona o operador diretamente para o PDV (UC08).
    

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor User as Usuário (Administrador / Operador)
    participant UI as Interface (Frontend)
    participant RBAC as Guard de Permissões (Roles)
    participant API as API Backend (DashboardService)
    participant DB as Banco de Dados

    User->>UI: Realiza Login no Sistema

    alt FA01: Acesso sem Privilégios (Operador de Caixa)
        UI->>RBAC: Verifica privilégios do perfil logado [RN05]
        RBAC-->>UI: Perfil "Operador" não possui permissão para métricas financeiras
        UI-->>User: Bloqueia dados financeiros, oculta Dashboard e redireciona ao PDV (UC08)
    else Fluxo Principal: Acesso Gerencial ao Dashboard [RF16]
        UI->>RBAC: Verifica privilégios do perfil logado
        RBAC-->>UI: Perfil "Gerente/Admin" autorizado
        UI-->>User: Direciona para o Dashboard Gerencial
        UI->>API: GET /api/v1/dashboard/metrics
        API->>DB: Calcula em tempo real (Faturamento do Dia, Top Produtos, Alertas Estoque) filtrado por Tenant
        DB-->>API: Retorna dados agregados da filial
        API-->>UI: Retorna métricas gerenciais
        UI-->>User: Renderiza gráficos e widgets interativos
    end
```