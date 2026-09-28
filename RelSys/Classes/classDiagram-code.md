```mermaid-next
---
  config:
    class:
      hideEmptyMembersBox: true
---
classDiagram
    namespace java.com.domain {
	    namespace auth {
		    namespace permissions {
			    class Permissions {
				    +name : String
			    }
		    }
		    
		    namespace roles {
			    class Roles {
				    +name : String
			    }
		    }
		    
		    namespace rolesPermissions {
			    class RolesPermissions {
				    +role : Roles
				    +permission : Permissions
			    }
		    }
		    
		    namespace users {
			    class Users {
				    +name : String
				    +role : Roles
				    +active : Boolean
			    }
		    }
	    }
	    
	    namespace financial {
		    namespace paymentMethod {
			    class PaymentMethod {
				    +name : String
				    +acronym : String
			    }
		    }
	    }
	    
	    namespace revenue {
		    namespace client {
			    class Client {
				    +person : Person
				    +personStatus : PersonStatus
			    }
		    }
		    
		    namespace person {
			    namespace enums.personStatus {
				    class PersonStatus {
					    +name : String
					    +getActive() PersonStatus
					    +getDeactive() PersonStatus
				    }
			    }
			    
			    namespace subtype {
				    namespace legalPerson {
					    class LegalPerson {
						    +cnpj : String
					    }
				    }
				    
				    namespace physicalPerson {
					    class PhysicalPerson {
						    +cpf : String
					    }
				    }
			    }
			    
			    class Person {
				    +name : String
				    +email : String
				    +personTypeAcronym : String
			    }
		    }
		    
		    namespace product {
			    namespace batch.productBatch {
				    namespace dtos {
					    class ProductBatchReceiveInputDto {
						    <<record>>
						    +productId : Long
						    +supplierId : Long
						    +purchasePrice : BigDecimal
						    +quantity : BigDecimal
					    }
					    
					    class ProductBatchReceiveOutputDto {
						    +id : Long
					    }
				    }
				    
				    namespace mappers {
					    class ProductBatchDtoMapper {
						    +toDto(entity : ProductBatch) ProductBatchReceiveOutputDto$
					    }
				    }
				    
				    namespace services {
					    class ProductBatchService {
						    +createProductBatch(productId : Long, supplierId : Long, purchasePrice : BigDecimal, purchaseQuantity : Long) ProductBatch
					    }
				    }
				    
				    class ProductBatch {
					    +productSupplier : ProductSupplier
					    +purchasePrice : BigDecimal
					    +purchaseQuantity : BigDecimal
					    +remainingQuantity : BigDecimal
					    +create(productSupplier : ProductSupplier, purchasePrice : BigDecimal, purchaseQuantity : BigDecimal) ProductBatch$
				    }
			    }
			    
			    namespace dtos {
				    class ProductInputDto {
					    <<record>>
					    name : String
					    supplierId : Long
					    barcode : String
					    price : BigDecimal
				    }
				    
				    class ProductOutputDto {
					    <<record>>
					    id : Long
					    suppliers : List~SupplierOutputDto~
					    name : String
					    price : BigDecimal
					    barcode : String
				    }
			    }
			    
			    namespace mappers {
				    class ProductDtoMapper {
					    toEntity(dto : ProductInputDto) Product$
					    toDto(product : Product) ProductOutputDto$
				    }
			    }
			    
			    namespace resources {
				    class ProductResource {
					    +getAllProducts()
					    +getProductByBarcode(barcode : String)
					    +createProduct(dto : ProductInputDto)
					    +createProductBatch(dto : productBatchReceiveInputDto)
				    }
			    }
			    
			    namespace suppliers.productSupplier {
				    namespace services {
					    class ProductSupplierService {
						    +createProduct(product : Product, supplierId : Long) Product$ 
						    +getAllProductAndSuppliers() Map~Product & List~Supplier~~
					    }
				    }
				    
				    class ProductSupplier {
					    +product : Product
					    +supplier : Supplier
					    +discountAllowed : Boolean
					    +create(product : Product, supplier : Supplier, discountAllowed : Boolean) ProductSupplier
					    +getAllProductSuppliers() List~ProductSupplier~
					    +getSuppliersOfProduct(product : Product) List~Supplier~
					    +findByIdTreated(id : Long) ProductSupplier
				    }
			    }
			    
			    class Product {
				    +name : String
				    +price
				    +barcode
				    +create(name : String, supplierId : Long, price : BigDecimal, barcode : String)
			    }
		    }
		    
		    namespace sale {
			    namespace enum.saleStatus {
				    class SaleStatus {
					    +name : String
				    }
			    }
			    
			    namespace productSale {
				    namespace productSaleBatch {
					    class ProductSaleBatch {
						    +productSale : ProductSale
						    +productBatch : ProductBatch
						    +quantity : BigDecimal
						    +applyDiscount : Boolean
						    +purchasePrice : BigDecimal
					    }
				    }
				    
				    class ProductSale {
					    +sale : Sale
					    +product : Product
					    +quantity : BigDecimal
					    +discountPercentage : BigDecimal
					    +productName : String
					    +productPrice : BigDecimal
				    }
			    }
			    
			    class Sale {
				    +client : Client
				    +paymentMethod : PaymentMethod
				    +discountPercentage : BigDecimal
				    +saleStatus : SaleStatus
			    }
		    }
		    
		    namespace supplier {
			    namespace dtos {
				    class SupplierInputDto {
					    <<record>>
					    +name : String
				    }
				    
				    class SupplierOutputDto {
					    <<record>>
					    +id : Long
					    +name : String
				    }
			    }
			    
			    namespace mappers {
				    class SupplierDtoMapper {
					    toEntity(dto : SupplierInputDto) Supplier
					    toDto(entity : Supplier) SupplierOutputDto
				    }
			    }
			    
			    namespace resource {
				    class SupplierResource {
					    +getAllSupplier()
					    +createSupplier(dto : SupplierInputDto)
				    }
			    }
			    
			    class Supplier {
				    +person : Person
				    +personStatus : PersonStatus
				    +create(name : String)
				    +findByIdTreated(id : Long)
			    }
		    }
	    }
	    
	    namespace shared {
		    class BaseEntity {
			    <<abstract>>
			    +id : Long
			    +createdAt : LocalDateTime
			    +updatedAt : LocalDateTime
		    }
	    }
    }
    
    %% AUTH
    Permissions --|> BaseEntity
    
    Roles --|> BaseEntity
    
    RolesPermissions --|> BaseEntity
    RolesPermissions o-- Permissions
    RolesPermissions o-- Roles
    
    Users --|> BaseEntity
    Users o-- Roles
    
    %% FINANCIAL
    PaymentMethod --|> BaseEntity
    
    %% REVENUE
    Client --|> BaseEntity
    Client o-- PersonStatus
    Client o-- Person
    
    PersonStatus --|> BaseEntity
    
    LegalPerson --|> Person
    
    PhysicalPerson --|> Person
    
    Person --|> BaseEntity
    
    ProductBatchDtoMapper ..> ProductBatch
    ProductBatchDtoMapper ..> ProductBatchReceiveOutputDto
    
    ProductBatchService ..> ProductBatch
    ProductBatchService ..> ProductSupplier
    
    ProductBatch o.. ProductSupplier
    ProductBatch --|> BaseEntity
    
    ProductOutputDto o-- SupplierOutputDto
    
    ProductDtoMapper ..> Product
    ProductDtoMapper ..> ProductOutputDto
    ProductDtoMapper ..> ProductInputDto
    
    ProductResource ..> ProductOutputDto
    ProductResource ..> ProductInputDto
    ProductResource ..> ProductDtoMapper
    ProductResource ..> ProductSupplierService
    ProductResource ..> ProductSupplier
    ProductResource ..> ProductBatchReceiveInputDto
    ProductResource ..> ProductBatchService
    ProductResource ..> ProductBatchReceiveOutputDto
    
    ProductSupplierService ..> Supplier
    ProductSupplierService ..> Product
    ProductSupplierService ..> ProductSupplier
    
    ProductSupplier ..> Product
    ProductSupplier ..> Supplier
    ProductSupplier ..> Person
    ProductSupplier --|> BaseEntity
    
    Product --|> BaseEntity
    
    SaleStatus --|> BaseEntity
    
    ProductSale --|> BaseEntity
    
    ProductSaleBatch --|> BaseEntity
    ProductSaleBatch o-- ProductBatch
    ProductSaleBatch o-- ProductSale
    
    ProductSale --|> BaseEntity
    ProductSale o-- Sale
    ProductSale o-- Product
    
    Sale --|> BaseEntity
    Sale o-- Client
    Sale o-- PaymentMethod
    Sale o-- SaleStatus
    
    SupplierDtoMapper ..> Supplier
    SupplierDtoMapper ..> SupplierOutputDto
    SupplierDtoMapper ..> SupplierInputDto
    
    SupplierResource ..> SupplierDtoMapper
    SupplierResource ..> SupplierOutputDto
    SupplierResource ..> SupplierInputDto
    
    Supplier --|> BaseEntity
    Supplier ..> Person
    Supplier ..> PersonStatus
    
    namespace java.com.infrastructure {
	    namespace exceptions {
		    class NotFoundException {
			    <<exception>>
		    }
		    
		    class DomainException {
			    <<exception>>
		    }
	    }
	    
	    namespace security.exceptionTreatment {
		    namespace dtos {
			    class ErrorResponseDto {
				    <<record>>
				    +errorMessage : String
				    +status : int
				    +timestamp : LocalDateTime
			    }
		    }
		    
		    class GenericExceptionMapper {
			    +toResponse(Throwable exception) Response
		    }
		    
		    class DomainExceptionMapper {
			    +toResponse(DomainException exception) Response
		    }
		    
		    class IllegalArgumentExceptionMapper {
			    +toResponse(IllegalArgumentException exception) Response
		    }
		    
		    class NotFoundExceptionMapper {
			    +toResponse(NotFoundException exception) Response
		    }
	    }
    }
    
    

```pro