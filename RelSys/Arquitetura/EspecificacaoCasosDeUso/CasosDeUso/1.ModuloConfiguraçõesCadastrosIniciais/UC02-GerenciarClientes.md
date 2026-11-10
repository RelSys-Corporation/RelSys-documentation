
UC02 - Gerenciar Clientes Objetivo: Criar e manter a base de clientes do estabelecimento filtrada pela empresa/filial atual. Requisitos Relacionados: [RF02], [RN04] 

Fluxo Principal (Registrar Novo Cliente):

1. O usuário acessa o menu lateral e clica em "Clientes" [RF02].
    
2. O sistema apresenta a listagem e o usuário clica em "Novo Cliente".
    
3. O sistema exibe um formulário e solicita a seleção do tipo: "Pessoa Física (CPF)" ou "Pessoa Jurídica (CNPJ)".
    
4. O usuário seleciona o tipo e preenche os dados obrigatórios.
    
5. O usuário clica em "Salvar".
    
6. O sistema valida a formatação do documento (CPF/CNPJ), grava os dados e exibe a mensagem (MSG04).

   Fluxos Alternativos:
    

- FA01 - Documento Inválido/Duplicado: No passo 6, se o CPF/CNPJ for inválido ou já existir na base, o sistema bloqueia e exibe a mensagem (MSG05).
    
- FA02 - Editar Cliente: O usuário altera informações e salva. O sistema atualiza o registro, mantendo o histórico de vendas antigas intacto, garantindo a imutabilidade [RN04]. O sistema exibe a mensagem (MSG06).Mensagens do Sistema (UC02):
    
- MSG04: "Cliente registrado com sucesso!"
    
- MSG05: "Documento inválido ou já registrado no sistema."
    
- MSG06: "Cliente atualizado com sucesso!"
    
    

```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
sequenceDiagram
    autonumber
    actor User as Usuário
    participant UI as Interface (Frontend)
    participant API as API Backend
    participant DB as Banco de Dados

    User->>UI: Acessa "Clientes" e clica em "Novo Cliente"
    UI-->>User: Exibe formulário e solicita tipo (PF/PJ)
    User->>UI: Seleciona tipo, preenche dados e CPF/CNPJ, clica em "Salvar"
    UI->>API: POST /api/v1/clients (Dados do Cliente)
    API->>API: Valida formatação do CPF/CNPJ

    alt Fluxo Principal: Registro com Sucesso
        API->>DB: Salva cliente associado ao Tenant atual
        DB-->>API: Registro gravado
        API-->>UI: Resposta de Sucesso (MSG04)
        UI-->>User: Exibe MSG04 e atualiza listagem
    else FA01: Documento Inválido ou Duplicado
        API->>DB: Verifica existência de CPF/CNPJ
        DB-->>API: Documento já existe ou formato inválido
        API-->>UI: Erro de Validação (MSG05)
        UI-->>User: Bloqueia salvamento e exibe MSG05
    else FA02: Editar Cliente Existente
        User->>UI: Altera informações do cliente e clica em "Salvar"
        UI->>API: PUT /api/v1/clients/{id} (Dados Atualizados)
        API->>DB: Atualiza cadastro do cliente (Imutabilidade de vendas [RN04])
        DB-->>API: Confirma atualização
        API-->>UI: Resposta de Sucesso (MSG06)
        UI-->>User: Exibe MSG06
    end
```