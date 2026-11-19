
Objetivo: Aplicar descontos, receber valores e baixar estoque por lote. Requisitos Relacionados: [RF02, RF13, RF14], [RN01, RN02] 

Fluxo Principal:

1. O operador clica em "Ir para Pagamento" na tela do PDV.
    
2. O operador vincula um cliente cadastrado [RF02] (opcional).
    
3. O operador seleciona o Método de Pagamento ativo [RF14].
    
4. O operador clica em "Confirmar Pagamento".
    
5. O sistema registra o recebimento financeiro na sessão de caixa atual.
    
6. O sistema deduz do estoque vinculando a venda aos lotes mais antigos / próximos do vencimento para cálculo de lucro (Controle PEPS/FEFO) [RN02].
    
7. O sistema chama automaticamente a emissão de comprovante (UC10).
    

Fluxos Alternativos:

- FA01 - Aplicar Desconto: O operador insere desconto [RF13]. O sistema valida a flag "Isenção de Desconto" [RN01]. Se isento, bloqueia o abatimento naquele item restrito e exibe a mensagem (MSG24).
    
- FA02 - Venda Avulsa: O operador não vincula nenhum cliente e finaliza como consumidor final.
    

Mensagens do Sistema (UC11):

- MSG24: "Desconto Bloqueado: O(s) produto(s) selecionado(s) possui(em) isenção de desconto configurada para o produto/fornecedor."


```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor Op as Operador
    participant UI as Interface (PDV)
    participant API as API Backend
    participant DB as Banco de Dados

    Op->>UI: Clica em "Ir para Pagamento"

    opt Vincular Cliente (Opcional - FA02 para Venda Avulsa)
        Op->>UI: Busca e vincula cliente cadastrado [RF02]
    end

    opt FA01: Aplicar Desconto
        Op->>UI: Insere desconto no item ou total [RF13]
        UI->>API: POST /api/v1/sales/validate-discount (Itens, Desconto)
        API->>DB: Verifica opção "Isenção de Desconto" (Produto/Fornecedor) [RN01]
        DB-->>API: Retorna status de isenção
        alt Produto/Fornecedor Isento
            API-->>UI: Desconto bloqueado (MSG24)
            UI-->>Op: Bloqueia abatimento no item restrito e exibe MSG24
        else Desconto Permitido
            API-->>UI: Desconto validado
            UI-->>Op: Atualiza valor líquido
        end
    end

    Op->>UI: Seleciona Método de Pagamento ativo [RF14] e clica em "Confirmar Pagamento"
    UI->>API: POST /api/v1/sales (Cliente, Itens, Descontos, MetodoPagamento)
    API->>DB: Grava venda concluída no histórico imutável [RN04]
    API->>DB: Registra recebimento financeiro na sessão de caixa atual
    API->>DB: Deduz estoque vinculando aos lotes mais antigos/próximos vencimento (PEPS/FEFO) [RN02]
    DB-->>API: Transação concluída com sucesso
    API-->>UI: Venda finalizada (Aciona UC10)
```