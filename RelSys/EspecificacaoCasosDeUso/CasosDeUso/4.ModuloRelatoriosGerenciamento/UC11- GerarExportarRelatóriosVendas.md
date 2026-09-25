
Objetivo: Extrair histórico e conferir faturamento. Requisitos Relacionados: [RF17, RF18], [RN04]

Fluxo Principal:

1. O usuário acessa "Relatórios" > "Histórico de Vendas" [RF17].
    
2. Preenche os filtros (Data, Cliente, Método) e clica em "Aplicar Filtros".
    
3. O sistema busca os dados no histórico imutável [RN04] e lista na tabela.
    
4. O usuário clica em "Exportar PDF" [RF18].
    
5. O sistema compila o PDF, inicia o download e exibe a mensagem (MSG20).
    

Fluxos Alternativos:

- FA01 - Busca Sem Resultados: Com filtros vazios, o sistema oculta a tabela, desabilita a exportação e exibe a mensagem (MSG21).
    
- FA02 - Visualizar Detalhes: O usuário clica sobre a venda na tabela para ver os itens, descontos e emitir 2ª via.
    

Mensagens do Sistema (UC11):

- MSG20: "Relatório de vendas gerado e exportado em PDF com sucesso."
    
- MSG21: "Nenhuma venda encontrada para o período selecionado."
    

**