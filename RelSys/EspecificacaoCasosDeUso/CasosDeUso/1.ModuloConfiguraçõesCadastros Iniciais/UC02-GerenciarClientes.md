
Especificação do Caso de Uso: UC02 - Gerenciar Clientes

**Fluxo Principal (Registrar Novo Cliente):**

1. O usuário acessa o menu lateral e clica em **"Clientes"**.
    
2. O sistema apresenta a listagem de clientes já registrados e um botão para novo registro.
    
3. O usuário clica em **"Novo Cliente"**.
    
4. O sistema exibe um formulário e solicita a seleção do tipo de cliente: "Pessoa Física (CPF)" ou "Pessoa Jurídica (CNPJ)".
    
5. O usuário seleciona o tipo e preenche os dados obrigatórios (ex: Nome/Razão Social, Documento, Contato, Endereço).
    
6. O usuário clica em **"Salvar"**.
    
7. O sistema valida a formatação do documento (CPF/CNPJ), grava os dados e exibe a mensagem: _"Cliente registrado com sucesso!"_.
    

**Fluxos Alternativos:**

- **FA01 - Documento Inválido ou Duplicado:** No passo 7, se o CPF ou CNPJ já existir na base de dados ou for matematicamente inválido, o sistema bloqueia o registro e exibe a mensagem: _"Documento inválido ou já registrado no sistema"_.
    
- **FA02 - Editar Cliente:** O usuário seleciona um cliente na listagem, clica em "Editar", altera as informações de contato ou endereço e clica em "Salvar". O sistema atualiza o registro mantendo o histórico intacto (RN04).