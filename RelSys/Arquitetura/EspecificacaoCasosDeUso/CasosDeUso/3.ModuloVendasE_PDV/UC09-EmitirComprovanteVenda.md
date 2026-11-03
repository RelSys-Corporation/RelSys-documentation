
Objetivo: Entregar a via física ou digital ao cliente. Requisitos Relacionados: [RF15], [RNF01]

Fluxo Principal:

1. Após a finalização da venda (via UC10), o sistema formata o layout do cupom não fiscal para bobinas de 58mm ou 80mm [RNF01].
    
2. O sistema envia a ordem para a impressora térmica [RF15].
    
3. A tela do PDV é resetada, preparada para a próxima venda e exibe a mensagem (MSG18).
    
4. Um botão "Imprimir 2ª Via" fica disponível na interface.
    

Fluxos Alternativos:

- FA01 - Falha na Impressora: A venda não é desfeita (pois já foi gravada). O usuário permite que a impressora normalize e clica em "Imprimir 2ª Via".
    

Mensagens do Sistema (UC09):

- MSG18: "Venda finalizada com sucesso!"
    

```mermaid-next
sequenceDiagram
    autonumber
    actor User as Usuario
    participant UI as Visão (PDV)
    participant Controller as Controlador (Comprovante)
    participant Printer as Serviço Impressora Térmica

    Note over UI,Controller: Chamado automaticamente após a finalização da venda (UC10)
    Controller->>Controller: formatarCupomNaoFiscal(dadosVenda, largura: 58mm/80mm) [RNF01]

    alt Fluxo Principal: Impressão com Sucesso
        Controller->>Printer: enviarOrdemImpressao(cupomFormatado) [RF15]
        Printer-->>Controller: impressaoConcluida
        Controller-->>UI: MSG18 ("Venda finalizada com sucesso!")
        UI->>UI: resetarTelaPDV()
        UI-->>User: Exibe MSG18, reseta PDV e disponibiliza botão "Imprimir 2ª Via"

    else FA01 - Falha na Impressora
        Controller->>Printer: enviarOrdemImpressao(cupomFormatado)
        Printer-->>Controller: erroConexao/Papel
        Note over Controller,UI: Venda NÃO é desfeita (já gravada)
        Controller-->>UI: notificarFalhaImpressao()
        UI-->>User: Exibe erro de impressão e mantém habilitado o botão "Imprimir 2ª Via"
        
        User->>Printer: Normaliza a impressora
        User->>UI: Clica em "Imprimir 2ª Via"
        UI->>Controller: reemitirComprovante(vendaId)
        Controller->>Printer: enviarOrdemImpressao(cupomFormatado)
        Printer-->>Controller: sucesso
        Controller-->>UI: impressaoConcluida
    end
```