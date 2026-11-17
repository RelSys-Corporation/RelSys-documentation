
Objetivo: Entregar a via física ou digital ao cliente. Requisitos Relacionados: [RF15], [RNF01] 

Fluxo Principal:

1. Após a finalização da venda (via UC11), o sistema formata o layout do cupom não fiscal para bobinas de 58mm ou 80mm [RNF01].
    
2. O sistema envia a ordem para a impressora térmica [RF15].
    
3. A tela do PDV é resetada, preparada para a próxima venda e exibe a mensagem (MSG23).
    
4. Um botão "Imprimir 2ª Via" fica disponível na interface.
    

Fluxos Alternativos:

- FA01 - Falha na Impressora: A venda não é desfeita (pois já foi gravada). O usuário regulariza a impressora e clica em "Imprimir 2ª Via".
    

Mensagens do Sistema (UC10):

- MSG23: "Venda finalizada com sucesso!"
    
    

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor Op as Operador
    participant UI as Interface (PDV)
    participant Printer as Impressora Térmica

    Note over Op,UI: Acionado automaticamente após finalização da venda (UC11)
    UI->>UI: Formata layout do cupom não fiscal (58mm ou 80mm) [RNF01]

    alt Fluxo Principal: Impressão com Sucesso
        UI->>Printer: Envia ordem de impressão térmica [RF15]
        Printer-->>UI: Impressão concluída
        UI-->>Op: Reseta tela do PDV, habilita "Imprimir 2ª Via" e exibe MSG23
    else FA01: Falha na Impressora
        UI->>Printer: Envia ordem de impressão térmica
        Printer--xUI: Erro de hardware / sem papel (venda permanece gravada)
        Op->>Printer: Regulariza a impressora
        Op->>UI: Clica em "Imprimir 2ª Via"
        UI->>Printer: Reenvia ordem de impressão do cupom
        Printer-->>UI: 2ª via impressa
    end
```