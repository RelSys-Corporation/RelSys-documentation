
UC10 - Emitir Comprovante de Venda Objetivo: Entregar a via física ou digital ao cliente. Requisitos Relacionados: [RF15], [RNF01] Fluxo Principal:

1. Após a finalização da venda (via UC11), o sistema formata o layout do cupom não fiscal para bobinas de 58mm ou 80mm [RNF01].
    
2. O sistema envia a ordem para a impressora térmica [RF15].
    
3. A tela do PDV é resetada, preparada para a próxima venda e exibe a mensagem (MSG20).
    
4. Um botão "Imprimir 2ª Via" fica disponível na interface.
    

Fluxos Alternativos:

- FA01 - Falha na Impressora: A venda não é desfeita (pois já foi gravada). O usuário permite que a impressora normalize e clica em "Imprimir 2ª Via".
    

Mensagens do Sistema (UC10): 

- MSG20: "Venda finalizada com sucesso!"
    

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    participant SaleProcess as Processo de Venda (UC11)
    participant UI as Interface (PDV)
    participant PrintService as Serviço de Layout/Impressão
    participant ThermalPrinter as Impressora Térmica

    SaleProcess->>UI: Venda finalizada com sucesso
    UI->>PrintService: Solicita formatação do cupom não fiscal [RNF01]
    PrintService->>PrintService: Formata layout para bobina de 58mm ou 80mm
    PrintService->>ThermalPrinter: Envia ordem de impressão [RF15]

    alt Fluxo Principal: Impressão com Sucesso
        ThermalPrinter-->>PrintService: Cupom impresso
        PrintService-->>UI: Confirmação de impressão
        UI-->>UI: Reseta tela do PDV para a próxima venda
        UI-->>UI: Habilita botão "Imprimir 2ª Via"
        UI-->>SaleProcess: Exibe MSG20
    else FA01: Falha na Impressora
        ThermalPrinter--xPrintService: Erro (sem papel / desconectada)
        PrintService-->>UI: Notifica falha de impressão (Venda mantida salva no banco)
        note over UI,ThermalPrinter: Operador normaliza a impressora
        UI->>PrintService: Operador clica em "Imprimir 2ª Via"
        PrintService->>ThermalPrinter: Reenvia ordem de impressão
        ThermalPrinter-->>PrintService: Cupom impresso
        PrintService-->>UI: Confirmação de 2ª via emitida
    end
```