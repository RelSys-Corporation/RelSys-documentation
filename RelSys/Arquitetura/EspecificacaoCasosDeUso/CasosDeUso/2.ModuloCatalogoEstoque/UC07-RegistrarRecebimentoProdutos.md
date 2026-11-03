
Objetivo: Dar entrada em mercadorias e definir preço de custo. Requisitos Relacionados: [RF10, RF11], [RN02]

Fluxo Principal:

1. O usuário acessa "Produtos" > "Recebimento" [RF10].
    
2. Seleciona o "Produto", informa o "R$ Preço de compra (unitário)" [RF11] e a "Quantidade".
    
3. O usuário clica em "Registrar Recebimento".
    
4. O sistema processa a entrada criando um novo lote distinto para suportar o cálculo de CMV [RN02].
    
5. O sistema atualiza o painel e exibe a mensagem (MSG14).
    

Fluxos Alternativos:

- FA01 - Campos Vazios: O sistema bloqueia a gravação e exibe a mensagem (MSG15).
    
- FA02 - Estado Inicial: Sem registros na sessão, exibe a mensagem (MSG16).
    

Mensagens do Sistema (UC07):

- MSG14: "Recebimento registrado com sucesso. Novo lote criado."
    
- MSG15: "Preencha todos os campos!"
    
- MSG16: "Nenhum recebimento registrado nesta sessão."
    

```mermaid-next
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Visão (Interface)
    participant Controller as Controlador (Estoque/Lotes)
    participant DB as Banco de Dados

    User->>UI: Acessa "Produtos" > "Recebimento" [RF10]

    opt FA02 - Estado Inicial
        UI-->>User: Exibe tela e MSG16 ("Nenhum recebimento registrado nesta sessão.")
    end

    alt Fluxo Principal: Registrar Entrada
        User->>UI: Seleciona "Produto", preenche "Preço de compra" e "Quantidade"
        User->>UI: Clica em "Registrar Recebimento"
        UI->>Controller: registrarEntrada(produtoId, precoCompra, quantidade)
        
        alt FA01 - Campos Vazios
            UI->>UI: validarCampos() [Incompletos]
            UI-->>User: Exibe MSG15 ("Preencha todos os campos!") e bloqueia
        else Campos Preenchidos
            Note over Controller,DB: Cria novo lote distinto para controle de CMV (PEPS) [RN02]
            Controller->>DB: INSERT INTO lotes (produto_id, preco_compra, quantidade, data_criacao)
            DB-->>Controller: loteCriado
            Controller->>DB: UPDATE produtos SET quantidade_total = quantidade_total + quantidade
            DB-->>Controller: sucesso
            Controller-->>UI: MSG14 ("Recebimento registrado com sucesso. Novo lote criado.")
            UI-->>User: Exibe MSG14 e atualiza painel da sessão
        end
    end
```