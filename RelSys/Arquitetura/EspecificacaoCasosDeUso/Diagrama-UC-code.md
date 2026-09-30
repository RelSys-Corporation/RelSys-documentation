
```mermaid-next
usecase-beta 
	direction LR 
	actor Usuario("Usuario") 
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
	
			UC08("Realizar Venda (Adicionar Produto no PDV)(UC08)")
			UC08include1("Consulta e Listagem de Produtos(UC05)")
			UC10("Registrar Pagamento e Finalizar Venda(UC10)")
			UC10extend1("Vincular Cliente à Venda")
			UC10extend2("Aplicar Desconto")
			UC10include1("Emitir Comprovante de Venda (chama UC09)")
			UC10include1extend("Imprimir 2ª Via de Comprovante(caso falha)")
			
	end 
	
	systemBoundary "Configurações e Gerencia"
	
			UC01("Parametrizar Métodos de Pagamento(UC01)")
			UC11("Gerar e Exportar Relatórios de Vendas(UC11)")
			UC11extend1("Visualizar Detalhes da Venda")
			UC11extend2("Imprimir 2ª Via de Comprovante")
			UC12("Visualizar Dashboard Gerencial(UC12)")
	
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
	UC08 ..> : include UC08include1
	Operador --> UC10
	UC10 ..> : extend UC10extend1
	UC10 ..> : extend UC10extend2
	UC10 ..> : include UC10include1
	UC10 ..> : extend UC10include1extend
	
	Administrador --> UC01
	Administrador --> UC11
	UC11 ..> : extend UC11extend1
	UC11 ..> : extend UC11extend2
	Administrador --> UC12
	
	Administrador --|> Usuario
	Administrador --|> Operador
	
```