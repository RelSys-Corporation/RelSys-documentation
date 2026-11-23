
```mermaid-next
%%{init: {'theme': 'redux-dark-color'}}%%
usecase-beta
direction LR

actor Usuario("Assistente Administrativo")
actor Operador("Operador")
actor Administrador("Administrador")

systemBoundary "Cadastros e Retaguarda"

UC02("Gerenciar Clientes(UC02)")
UC03("Gerenciar Fornecedores(UC03)")
UC04("Cadastro de Produto(UC04)")
UC04extnd1("Gerar Código de Barras Interno")
UC05("Consulta e Listagem de Produtos(UC05)")
UC06("Gerar e Imprimir Etiquetas(UC06)")
UC07("Registrar Recebimento de Produtos(UC07)")

end

systemBoundary "Frente de Caixa / PDV"

UC09("Realizar Venda (Adicionar Produto no PDV)(UC09)")
UC05include("Consulta e Listagem de Produtos(UC05)")
UC08("Gerenciar Sessão do Caixa(UC08)")
UC11("Registrar Pagamento e Finalizar Venda(UC11)")
UC11extend1("Vincular Cliente à Venda")
UC11extend2("Aplicar Desconto")
UC10("Emitir Comprovante de Venda(UC10)")
UC10extend1("Imprimir 2ª Via de Comprovante")

end

systemBoundary "Configurações e Gerência"

UC01("Parametrizar Métodos de Pagamento(UC01)")
UC13("Gerar e Exportar Relatórios de Vendas(UC13)")
UC13extend1("Visualizar Detalhes da Venda")
UC13extend2("Imprimir 2ª Via de Comprovante")
UC14("Visualizar Dashboard Gerencial(UC14)")
UC12("Cancelar Venda e Estornar Operação(UC12)")
UC15("Gerenciar Perfis e permissões(UC15)")
UC16("Gerenciar Usuarios(UC16)")

end

Usuario --> UC02
Operador --> UC02
Usuario --> UC03
Usuario --> UC04
UC04 ..> : extend UC04extnd1
Usuario --> UC05
Usuario --> UC06
Usuario --> UC07



Operador --> UC08
UC08 ..> : extend UC09
UC09 ..> : include UC05include

Operador --> UC11
UC11 ..> : extend UC11extend1
UC11 ..> : extend UC11extend2
UC11 ..> : include UC10
UC10 ..> : extend UC10extend1

Administrador --> UC01
Administrador --> UC13
UC13 ..> : extend UC13extend1
UC13 ..> : extend UC13extend2
Administrador --> UC14
Administrador --> UC12
Administrador --> UC15
Administrador --> UC16

Administrador --|> Usuario
Administrador --|> Operador

```