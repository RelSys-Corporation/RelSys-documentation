
UC12 - Gerar e Exportar Relatórios de Vendas Objetivo: Extrair histórico e conferir faturamento. Requisitos Relacionados: [RF17, RF18], [RN04] Fluxo Principal:

1. O usuário acessa "Relatórios" > "Histórico de Vendas" [RF17].
    
2. Preenche os filtros (Data, Cliente, Método) e clica em "Aplicar Filtros".
    
3. O sistema busca os dados no histórico imutável [RN04], filtrando automaticamente pela empresa/filial atual (Tenant), e lista na tabela.
    
4. O usuário clica em "Exportar PDF" [RF18].
    
5. O sistema compila o PDF, inicia o download e exibe a mensagem (MSG22).
    

Fluxos Alternativos:

- FA01 - Busca Sem Resultados: Com filtros vazios, o sistema oculta a tabela, desabilita a exportação e exibe a mensagem (MSG23).
    
- FA02 - Visualizar Detalhes: O usuário clica sobre a venda na tabela para ver os itens, descontos e emitir 2ª via.
    

Mensagens do Sistema (UC12):

- MSG22: "Relatório de vendas gerado e exportado em PDF com sucesso."

- MSG23: "Nenhuma venda encontrada para o período selecionado."
    

```mermaid-next
sequenceDiagram
    autonumber
    actor User as Usuário / Administrador
    participant UI as Interface (Frontend)
    participant API as API Backend (ReportService)
    participant DB as Banco de Dados
    participant PDFEngine as Gerador de PDF

    User->>UI: Acessa "Relatórios" > "Histórico de Vendas" [RF17]
    User->>UI: Preenche filtros (Data, Cliente, Método) e clica em "Aplicar Filtros"
    UI->>API: GET /api/v1/reports/sales?startDate={d1}&endDate={d2}&client={c}&method={m}
    API->>DB: Consulta vendas no histórico imutável [RN04] (filtrado por Tenant/Filial)

    alt Fluxo Principal: Exibição e Exportação em PDF
        DB-->>API: Retorna registros de vendas
        API-->>UI: Retorna dados estruturados
        UI-->>User: Lista vendas na tabela de relatórios
        User->>UI: Clica em "Exportar PDF" [RF18]
        UI->>API: POST /api/v1/reports/sales/export-pdf (Filtros)
        API->>PDFEngine: Compila dados em layout PDF
        PDFEngine-->>API: Retorna arquivo PDF binário
        API-->>UI: Envia download do arquivo PDF
        UI-->>User: Inicia download do arquivo e exibe MSG22
    else FA01: Busca Sem Resultados
        DB-->>API: Nenhum registro encontrado para o período/filtro
        API-->>UI: Retorna lista vazia
        UI-->>User: Oculta tabela, desabilita exportação e exibe MSG23
    else FA02: Visualizar Detalhes da Venda
        User->>UI: Clica sobre uma linha de venda na tabela
        UI->>API: GET /api/v1/sales/{saleId}/details
        API->>DB: Busca itens, descontos e métodos da venda
        DB-->>API: Retorna detalhes da venda
        API-->>UI: Exibe modal com detalhes e botão de 2ª via do comprovante
    end
```