
Objetivo: Extrair histórico imutável de vendas (concluídas e canceladas) e conferir faturamento. Requisitos Relacionados: [RF17, RF19, RF20], [RN04] 
Fluxo Principal:

1. O usuário acessa "Relatórios" > "Histórico de Vendas" [RF19].
    
2. Preenche os filtros (Data, Cliente, Método, Status) e clica em "Aplicar Filtros".
    
3. O sistema busca os dados no histórico imutável [RN04], filtrando automaticamente pela empresa/filial atual, e lista na tabela (destacando visualmente vendas com status Cancelada).
    
4. O usuário clica em "Exportar PDF" [RF20].
    
5. O sistema compila o PDF, inicia o download e exibe a mensagem (MSG29).
    

Fluxos Alternativos:

- FA01 - Busca Sem Resultados: Com filtros sem correspondência, o sistema oculta a tabela, desabilita a exportação e exibe a mensagem (MSG30).
    
- FA02 - Visualizar Detalhes e Acionar Cancelamento: O usuário clica sobre a venda na tabela para ver os itens, descontos, emitir 2ª via ou acionar o cancelamento [RF17] (UC12).
    

Mensagens do Sistema (UC13):

- MSG29: "Relatório de vendas gerado e exportado em PDF com sucesso."
    
- MSG30: "Nenhuma venda encontrada para o período selecionado."
    
    
```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    User->>UI: Acessa "Relatórios" > "Histórico de Vendas" [RF19]
    User->>UI: Preenche filtros (Data, Cliente, Método, Status) e clica em "Aplicar Filtros"
    UI->>API: GET /api/v1/reports/sales?filtros
    API->>DB: Busca histórico imutável [RN04] filtrado pela empresa/filial atual
    DB-->>API: Retorna vendas (concluídas e canceladas)

    alt Fluxo Principal: Exibir e Exportar Relatório
        API-->>UI: Lista vendas na tabela (destacando visualmente status Cancelada)
        opt FA02: Visualizar Detalhes e Acionar Cancelamento
            User->>UI: Clica sobre uma venda na tabela
            UI-->>User: Exibe itens, descontos, emitir 2ª via ou acionar cancelamento [RF17] (UC12)
        end
        User->>UI: Clica em "Exportar PDF" [RF20]
        UI->>API: GET /api/v1/reports/sales/export-pdf?filtros
        API->>API: Compila documento PDF
        API-->>UI: Retorna arquivo binário PDF e MSG29
        UI-->>User: Inicia download do PDF e exibe MSG29
    else FA01: Busca Sem Resultados
        API-->>UI: Retorna lista vazia
        UI-->>User: Oculta tabela, desabilita exportação e exibe MSG30
    end
```