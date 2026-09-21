### Especificação do Caso de Uso: UC12 - Visualizar Dashboard Gerencial

**Ator Principal:** Administrador ou Gerente **Requisitos Atendidos:** RF16 **Objetivo:** Apresentar um painel de controle central com os principais indicadores de desempenho do estabelecimento logo após o início de sessão.

**Pré-condições:** O usuário deve ter o perfil de gestão com permissões para visualizar métricas financeiras.

**Fluxo Principal:**

1. O usuário faz o login no sistema.
    
2. O sistema direciona automaticamente o usuário para a tela inicial (**Dashboard**).
    
3. O sistema calcula em tempo real e exibe os seguintes blocos de informação (Widgets):
    
    - **Faturamento do Dia:** Valor total vendido no dia corrente.
        
    - **Produtos Mais Vendidos:** Um gráfico ou lista com os itens de maior saída no mês.
        
    - **Alertas de Estoque Baixo:** Uma lista de produtos cuja quantidade em inventário atingiu a margem mínima de segurança.
        
4. O usuário interage com os gráficos (ex: passando o mouse por cima para ver valores exatos) para analisar a operação.
    

**Fluxos Alternativos:**

- **FA01 - Acesso sem Privilégios:** Se um Operador de Caixa (sem permissão de gestão) fizer login, o sistema oculta o Dashboard financeiro e direciona o usuário diretamente para a frente de caixa (PDV) ou exibe um painel simplificado sem dados monetários.