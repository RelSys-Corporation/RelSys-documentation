
UC11 - Registrar Pagamento e Finalizar Venda Objetivo: Aplicar descontos, receber valores e baixar estoque. Requisitos Relacionados: [RF02, RF13, RF14], [RN01, RN02] Fluxo Principal:

1. O operador clica em "Ir para Pagamento" na tela do PDV.
    
2. O operador vincula um cliente cadastrado [RF02] (opcional).
    
3. O operador seleciona o Método de Pagamento [RF14].
    
4. O operador clica em "Confirmar Pagamento".
    
5. O sistema registra o pagamento na sessão de caixa atual (CashProcess).
    
6. O sistema deduz do estoque vinculando a venda ao lote mais antigo para cálculo de lucro (Controle PEPS) [RN02].
    
7. O sistema chama automaticamente a emissão de comprovante (UC10).
    

Fluxos Alternativos:

- FA01 - Aplicar Desconto: O operador insere desconto [RF13]. O sistema valida a flag "Isenção de Desconto" [RN01]. Se isento, bloqueia o abatimento naquele item restrito e exibe a mensagem (MSG21).
    
- FA02 - Venda Avulsa: O operador não vincula nenhum cliente e finaliza como consumidor final.
    

Mensagens do Sistema (UC11):

- MSG21: "Desconto Bloqueado: O(s) produto(s) selecionado(s) possui(em) isenção de desconto configurada para o fornecedor atual."
    

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor Operator as Operador
    participant UI as Interface (PDV)
    participant API as API Backend (Sale/Stock/Cash)
    participant DB as Banco de Dados
    participant UC10 as Emissão de Comprovante (UC10)

    Operator->>UI: Clica em "Ir para Pagamento" no PDV

    opt FA02: Venda Avulsa / Vincular Cliente
        alt Vincular Cliente
            Operator->>UI: Busca e seleciona cliente cadastrado [RF02]
        else Venda Avulsa
            note over UI: Finaliza como Consumidor Final (Sem cliente)
        end
    end

    opt FA01: Aplicação de Desconto
        Operator->>UI: Insere valor/percentual de desconto [RF13]
        UI->>API: POST /api/v1/sales/validate-discount (Itens, Desconto)
        API->>DB: Verifica flag "Isenção de Desconto" do produto/fornecedor [RN01]
        alt Produto possui isenção de desconto
            DB-->>API: Retorna restrição ativa
            API-->>UI: Rejeita desconto (MSG21)
            UI-->>Operator: Exibe mensagem de bloqueio (MSG21)
        end
    end

    Operator->>UI: Seleciona "Método de Pagamento" [RF14] e clica em "Confirmar Pagamento"
    UI->>API: POST /api/v1/sales/finalize (Dados da Venda, Pagamento, Cliente)
    API->>DB: Registra pagamento na sessão de caixa atual (CashProcess)
    API->>DB: Deduz do estoque pelo lote mais antigo - PEPS [RN02]
    API->>DB: Grava venda no histórico imutável [RN04]
    DB-->>API: Confirmação da transação
    API-->>UI: Retorna Venda Finalizada
    UI->>UC10: Aciona automaticamente a emissão do comprovante (UC10)
```