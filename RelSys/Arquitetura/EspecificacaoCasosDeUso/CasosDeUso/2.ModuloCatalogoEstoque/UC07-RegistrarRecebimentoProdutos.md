
Objetivo: Dar entrada em mercadorias definindo preço de custo e data de validade do lote. Requisitos Relacionados: [RF10, RF11], [RN02] 

Fluxo Principal:

1. O usuário acessa "Produtos" > "Recebimento" [RF10].
    
2. Seleciona o "Produto", informa o "R$ Preço de compra (unitário)" [RF11], a "Quantidade" e a "Data de Validade" do lote (opcional para itens não perecíveis).
    
3. O usuário clica em "Registrar Recebimento".
    
4. O sistema processa a entrada criando um novo lote distinto com seu custo unitário e validade para suportar o cálculo de CMV e alertas de vencimento [RN02].
    
5. O sistema atualiza o painel e exibe a mensagem (MSG14).
    

Fluxos Alternativos:

- FA01 - Campos Vazios: O usuário deixa campos obrigatórios em branco. O sistema bloqueia a gravação e exibe a mensagem (MSG15).
    
- FA02 - Data de Validade Inválida: O usuário informa uma data de validade anterior à data atual. O sistema bloqueia o registro e exibe a mensagem (MSG16).
    
- FA03 - Estado Inicial: Sem registros na sessão, exibe a mensagem (MSG17).
    

Mensagens do Sistema (UC07):

- MSG14: "Recebimento registrado com sucesso. Novo lote criado."
    
- MSG15: "Preencha todos os campos obrigatórios!"
    
- MSG16: "A data de validade não pode ser anterior à data atual."
    
- MSG17: "Nenhum recebimento registrado nesta sessão."
    

**
```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    User->>UI: Acessa "Produtos" > "Recebimento" [RF10]
    UI-->>User: Exibe formulário e MSG17 (FA03 - Estado Inicial)

    alt Fluxo Principal: Registrar Recebimento
        User->>UI: Seleciona Produto, informa Preço compra [RF11], Qtd e Data Validade
        User->>UI: Clica em "Registrar Recebimento"
        UI->>API: POST /api/v1/stock/receive (Dados do Lote)
        API->>API: Valida campos e data de validade (>= data atual)
        API->>DB: Cria novo lote distinto com custo e validade [RN02]
        DB-->>API: Confirma criação do lote
        API-->>UI: Sucesso (MSG14)
        UI-->>User: Atualiza painel e exibe MSG14
    else FA01: Campos Vazios
        User->>UI: Submete com campos obrigatórios em branco
        UI-->>User: Bloqueia gravação e exibe MSG15
    else FA02: Data de Validade Inválida
        User->>UI: Informa data de validade anterior à data atual
        UI->>API: POST /api/v1/stock/receive (Data passada)
        API-->>UI: Erro de validação (MSG16)
        UI-->>User: Bloqueia registro e exibe MSG16
    end

```