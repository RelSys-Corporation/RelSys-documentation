
Objetivo: Prover visão rápida da saúde do negócio ao acessar o sistema. Requisitos Relacionados: [RF16], [RN05]

Fluxo Principal:

1. O usuário faz login no sistema.
    
2. O sistema direciona o gerente para o Dashboard [RF16].
    
3. O sistema calcula em tempo real os widgets de: Faturamento do Dia, Produtos Mais Vendidos e Alertas de Estoque Baixo.
    
4. O usuário interage com os gráficos.
    

Fluxos Alternativos:

- FA01 - Acesso sem Privilégios: Um Operador de Caixa faz login. Aplicando o Princípio do Menor Privilégio [RN05], o sistema bloqueia os dados financeiros, oculta o Dashboard e redireciona o operador diretamente para o PDV (Frente de Caixa).
    

```mermaid-next
sequenceDiagram
    autonumber
    actor User as Administrador
    participant Auth as Sistema Autenticação / Router
    participant UI as Visão (Dashboard / PDV)
    participant Controller as Controlador (Dashboard)
    participant DB as Banco de Dados

    User->>Auth: Faz login no sistema

    alt FA01 - Acesso sem Privilégios (Operador de Caixa)
        Auth->>Auth: verificarPermissoes(usuario) [RN05]
        Note over Auth,UI: Princípio do Menor Privilégio: bloqueia dados financeiros
        Auth-->>UI: Redireciona para Frente de Caixa (PDV)
        UI-->>User: Renderiza tela do PDV (oculta Dashboard)

    else Fluxo Principal: Acesso com Privilégios (Gerente)
        Auth->>Auth: verificarPermissoes(usuario) [Permitido]
        Auth->>UI: Direciona para o Dashboard [RF16]
        UI->>Controller: carregarDadosDashboard()
        
        par Cálculo dos Widgets em Tempo Real
            Controller->>DB: SUM(valor_total) WHERE data = HOJE
            DB-->>Controller: faturamentoDia
        and
            Controller->>DB: SELECT produtos ORDER BY quantidade_vendida DESC LIMIT X
            DB-->>Controller: maisVendidos
        and
            Controller->>DB: SELECT * FROM produtos WHERE quantidade <= estoque_minimo
            DB-->>Controller: alertasEstoque
        end
        
        Controller-->>UI: renderizarDashboard(faturamento, maisVendidos, alertas)
        UI-->>User: Exibe Dashboard interativo com gráficos e widgets
    end
```