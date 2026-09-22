
Especificação do Caso de Uso: UC11 - Gerar e Exportar Relatórios de Vendas Fluxo Principal (Filtrar Histórico e Exportar PDF):

1. O usuário acessa o menu lateral, expande a seção "Relatórios" (ou "Vendas") e clica em "Histórico de Vendas".
    
2. O sistema exibe a interface contendo um painel de filtros no topo e uma tabela com as vendas do dia atual listadas abaixo.
    
3. O usuário preenche os campos de filtro desejados, como "Data Inicial", "Data Final", "Cliente" ou "Método de Pagamento" (atendendo ao RF17).
    
4. O usuário clica no botão azul "Aplicar Filtros".
    
5. O sistema consulta o histórico imutável (RN04) e atualiza a tabela, exibindo apenas as transações correspondentes aos critérios informados.
    
6. O usuário clica no botão branco "Exportar PDF", localizado na parte superior direita da tabela (atendendo ao RF18).
    
7. O sistema compila os dados da listagem atual e gera um documento em formato PDF com cabeçalho gerencial e consolidação dos valores (ex: soma total do período filtrado).
    
8. O sistema inicia automaticamente o download do arquivo PDF para o dispositivo do usuário.
    

Fluxos Alternativos:

- FA01 - Busca Sem Resultados (Período Vazio): O usuário aplica filtros sem resultados. O sistema exibe "Nenhuma venda encontrada para o período selecionado" e desabilita o botão "Exportar PDF".
    
- FA02 - Visualizar Detalhes da Venda Específica: O usuário clica sobre uma linha de venda. O sistema abre uma janela (modal) com o detalhamento completo (produtos, lotes, operador, descontos) e a opção de "Imprimir 2ª Via".
    
- FA03 - Exportação sem Filtros (Visão Padrão): O usuário ignora os filtros e clica em "Exportar PDF". O sistema gera o relatório do período padrão (faturamento do dia corrente).
    

  
**