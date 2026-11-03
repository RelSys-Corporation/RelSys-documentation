
Objetivo: Criar identificação física (código de barras) para as mercadorias. Requisitos Relacionados: [RF08], [RNF02]

Fluxo Principal:

1. O usuário acessa o menu e clica em "Etiquetas" [RF08].
    
2. Seleciona o produto, informa a quantidade e clica em "+ Adicionar".
    
3. O sistema atualiza a "Fila de Impressão".
    
4. O usuário clica em "Imprimir Etiquetas".
    
5. O sistema gera o arquivo compatível com impressoras térmicas (ZPL/PPLA) ou PDF (A4) [RNF02] e envia para impressão.
    

Fluxos Alternativos:

- FA01 - Fila Vazia: Ao acessar, a fila exibe "0 produtos" e a mensagem (MSG13).
    
- FA02 - Remover Item: O usuário clica no "X" e o item sai da fila.
    

Mensagens do Sistema (UC06):

- MSG13: "Adicione produtos para gerar etiquetas."
    

```mermaid-next
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Visão (Interface)
    participant Controller as Controlador (Etiquetas)
    participant PrintService as Serviço de Impressão

    User->>UI: Acessa menu "Etiquetas" [RF08]
    UI->>Controller: obterFilaImpressao()

    opt FA01 - Fila Vazia
        Controller-->>UI: filaVazia
        UI-->>User: Exibe "0 produtos" e MSG13 ("Adicione produtos para gerar etiquetas.")
    end

    User->>UI: Seleciona produto, informa quantidade e clica em "+ Adicionar"
    UI->>Controller: adicionarAFila(produtoId, quantidade)
    Controller-->>UI: filaAtualizada
    UI-->>User: Exibe tabela da Fila de Impressão atualizada

    opt FA02 - Remover Item
        User->>UI: Clica no "X" de um item
        UI->>Controller: removerDaFila(produtoId)
        Controller-->>UI: filaAtualizada
        UI-->>User: Remove item da exibição na fila
    end

    alt Fluxo Principal: Imprimir Etiquetas
        User->>UI: Clica em "Imprimir Etiquetas"
        UI->>Controller: gerarEtiquetas(fila)
        Controller->>Controller: formatarLayout(ZPL/PPLA ou PDF A4) [RNF02]
        Controller->>PrintService: enviarOrdemImpressao(arquivo)
        PrintService-->>Controller: sucesso
        Controller-->>UI: impressaoIniciada
        UI-->>User: Notifica envio para impressora
    end
```