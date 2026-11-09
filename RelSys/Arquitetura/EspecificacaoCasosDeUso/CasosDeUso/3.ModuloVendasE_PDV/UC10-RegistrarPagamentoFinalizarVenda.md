
Objetivo: Aplicar descontos, receber valores e baixar estoque. Requisitos Relacionados: [RF02, RF13, RF14], [RN01, RN02]

Fluxo Principal:

1. O usuário clica em "Ir para Pagamento" na tela do PDV.
    
2. O usuário vincula um cliente cadastrado [RF02] (opcional).
    
3. O usuário seleciona o Método de Pagamento [RF14].
    
4. O usuário clica em "Confirmar Pagamento".
    
5. O sistema deduz do estoque o lote mais antigo para cálculo de lucro (Controle PEPS) [RN02].
    
6. O sistema chama automaticamente a emissão de comprovante (UC09).
    

Fluxos Alternativos:

- FA01 - Aplicar Desconto: O operador insere desconto [RF13]. O sistema valida a flag "Isenção de Desconto" [RN01]. Se isento, bloqueia o abatimento naquele item restrito e exibe a mensagem (MSG19).
    
- FA02 - Venda Avulsa: O operador não vincula nenhum cliente e finaliza como consumidor final.
    

Mensagens do Sistema (UC10):

- MSG19: "Desconto Bloqueado: O(s) produto(s) selecionado(s) possui(em) isenção de desconto configurada para o fornecedor atual."
    

```mermaid-next
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Visão (PDV)
    participant Controller as Controlador (Vendas)
    participant DB as Banco de Dados

    User->>UI: Clica em "Ir para Pagamento" na tela do PDV
    UI-->>User: Exibe opções de cliente e pagamento

    alt FA02 - Venda Avulsa
        Note over User,UI: Operador não seleciona cliente (Consumidor Final)
    else Vincular Cliente
        User->>UI: Seleciona cliente cadastrado [RF02]
    end

    opt FA01 - Aplicar Desconto (Com validação de Isenção)
        User->>UI: Insere valor de desconto [RF13]
        UI->>Controller: validarDesconto(itensCarrinho)
        Controller->>DB: SELECT p.id, f.isento FROM produtos p JOIN fornecedores f ON p.fornecedor_id = f.id WHERE p.id IN (...)
        DB-->>Controller: dadosIsencao
        
        opt Produto com Isenção Ativa [RN01]
            Controller-->>UI: MSG19 ("Desconto Bloqueado: O(s) produto(s)...")
            UI-->>User: Exibe MSG19 e bloqueia o desconto no item restrito
        end
    end

    User->>UI: Seleciona Método de Pagamento [RF14] e clica em "Confirmar Pagamento"
    UI->>Controller: finalizarVenda(dadosVenda, dadosPagamento)

    Controller->>DB: SELECT * FROM lotes WHERE produto_id = id AND quantidade > 0 ORDER BY data_criacao ASC
    DB-->>Controller: lotesAntigos (Controle PEPS) [RN02]

    Controller->>DB: UPDATE lotes SET quantidade = quantidade - quantidadeVendida [RN02]
    Controller->>DB: INSERT INTO vendas (dadosVenda)
    Controller->>DB: INSERT INTO itens_venda (itens)
    DB-->>Controller: vendaGravada

    Controller->>Controller: invocarEmissaoComprovante() (UC09)
    Controller-->>UI: confirmaVendaEChamaUC09()
```