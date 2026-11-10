
UC07 - Registrar Recebimento de Produtos Objetivo: Dar entrada em mercadorias e definir preço de custo. Requisitos Relacionados: [RF10, RF11], [RN02] Fluxo Principal:

1. O usuário acessa "Produtos" > "Recebimento" [RF10].
    
2. Seleciona o "Produto", informa o "R$ Preço de compra (unitário)" [RF11] e a "Quantidade".
    
3. O usuário clica em "Registrar Recebimento".
    
4. O sistema processa a entrada criando um novo lote (ProductBatch) distinto para suportar o cálculo de CMV [RN02].
    
5. O sistema atualiza o painel e exibe a mensagem (MSG14).
    

Fluxos Alternativos:

- FA01 - Campos Vazios: O sistema bloqueia a gravação e exibe a mensagem (MSG15).
    
- FA02 - Estado Inicial: Sem registros na sessão, exibe a mensagem (MSG16).
    

Mensagens do Sistema (UC07): 

- MSG14: "Recebimento registrado com sucesso. Novo lote criado." 

- MSG15: "Preencha todos os campos!" 

- MSG16: "Nenhum recebimento registrado nesta sessão."

**
```mermaid-next
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Interface (Frontend)
    participant API as API Backend (StockService)
    participant DB as Banco de Dados

    User->>UI: Acessa "Produtos" > "Recebimento" [RF10]

    alt FA02: Estado Inicial da Sessão
        UI-->>User: Exibe painel inicial com MSG16
    end

    User->>UI: Seleciona "Produto", informa "Preço de Compra" e "Quantidade"
    User->>UI: Clica em "Registrar Recebimento

    alt Fluxo Principal: Registro de Entrada de Lote
        UI->>API: POST /api/v1/stock/receive (ProductID, CostPrice, Qty)
        API->>DB: Cria novo lote (ProductBatch) distinto para cálculo de CMV [RN02]
        DB-->>API: Confirma criação do lote
        API-->>UI: Retorna Sucesso (MSG14)
        UI-->>User: Atualiza painel de recebimentos e exibe MSG14
    else FA01: Campos Obrigatórios Vazios
        User->>UI: Clica em "Registrar Recebimento" com campos em branco
        UI-->>User: Bloqueia gravação e exibe MSG15
    end


```