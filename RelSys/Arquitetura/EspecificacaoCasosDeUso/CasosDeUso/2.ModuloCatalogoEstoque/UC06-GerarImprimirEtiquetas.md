
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
    participant UI as Interface (Frontend)
    participant PrintService as Serviço de Impressão
    participant Printer as Impressora

    User->>UI: Acessa menu "Etiquetas" [RF08]

    alt FA01: Fila Vazia no Acesso
        UI-->>User: Exibe fila com "0 produtos" e exibe MSG13
    end

    User->>UI: Seleciona produto, informa quantidade e clica em "+ Adicionar"
    UI-->>User: Atualiza Fila de Impressão na tela

    opt FA02: Remover Item da Fila
        User->>UI: Clica no "X" ao lado do item
        UI-->>User: Remove item da Fila de Impressão
    end

    User->>UI: Clica em "Imprimir Etiquetas"
    UI->>PrintService: Solicita geração de etiquetas (ZPL/PPLA ou PDF A4) [RNF02]
    PrintService->>Printer: Envia arquivo gerado para impressão
    Printer-->>PrintService: Confirma recebimento do trabalho
    PrintService-->>UI: Impressão iniciada
    UI-->>User: Notifica envio para impressão
```