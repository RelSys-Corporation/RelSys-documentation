
Objetivo: Criar e manter a base de clientes do estabelecimento filtrada pela empresa/filial atual. Requisitos Relacionados: [RF02], [RN04], [RNF03, RNF05] 

Fluxo Principal (Registrar Novo Cliente):

1. O usuário acessa o menu lateral e clica em "Clientes" [RF02].
    
2. O sistema apresenta a listagem da filial atual [RNF05] e o usuário clica em "Novo Cliente".
    
3. O sistema exibe um formulário e solicita a seleção do tipo: "Pessoa Física (CPF)" ou "Pessoa Jurídica (CNPJ)".
    
4. O usuário seleciona o tipo e preenche os dados obrigatórios.
    
5. O usuário clica em "Salvar".
    
6. O sistema valida a formatação matemática do documento (CPF/CNPJ), grava os dados e exibe a mensagem (MSG04).
    

Fluxos Alternativos:

- FA01 - Documento Inválido/Duplicado: No passo 6, se o CPF/CNPJ for inválido ou já existir na base da filial, o sistema bloqueia e exibe a mensagem (MSG05).
    
- FA02 - Editar Cliente: O usuário altera informações e salva. O sistema atualiza o registro, mantendo o histórico de vendas antigas intacto, garantindo a imutabilidade [RN04]. O sistema exibe a mensagem (MSG06).
    

Mensagens do Sistema (UC02):

- MSG04: "Cliente registrado com sucesso!"
    
- MSG05: "Documento inválido ou já registrado no sistema."
    
- MSG06: "Cliente atualizado com sucesso!"
    

**

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    User->>UI: Acessa menu lateral > "Clientes" [RF02]
    UI->>API: GET /api/v1/clients
    API->>DB: Consulta clientes da filial atual [RNF05]
    DB-->>API: Retorna listagem
    API-->>UI: Exibe listagem de clientes

    alt Fluxo Principal: Registrar Novo Cliente
        User->>UI: Clica em "Novo Cliente"
        UI-->>User: Exibe formulário (PF/CPF ou PJ/CNPJ)
        User->>UI: Seleciona tipo, preenche dados e clica em "Salvar"
        UI->>API: POST /api/v1/clients (Dados)
        API->>API: Valida formatação matemática do CPF/CNPJ
        API->>DB: Verifica unicidade e grava cliente na filial
        DB-->>API: Confirma gravação
        API-->>UI: Sucesso (MSG04)
        UI-->>User: Exibe MSG04 e atualiza listagem
    else FA01: Documento Inválido ou Duplicado
        User->>UI: Submete CPF/CNPJ inválido ou já existente
        UI->>API: POST /api/v1/clients (Dados)
        API->>DB: Verifica existência do documento na filial
        DB-->>API: Documento inválido ou duplicado
        API-->>UI: Erro de validação (MSG05)
        UI-->>User: Bloqueia cadastro e exibe MSG05
    else FA02: Editar Cliente
        User->>UI: Altera informações e clica em "Salvar"
        UI->>API: PUT /api/v1/clients/{id} (Dados)
        API->>DB: Atualiza cadastro (histórico de vendas intacto [RN04])
        DB-->>API: Confirma atualização
        API-->>UI: Sucesso (MSG06)
        UI-->>User: Exibe MSG06
    end
```