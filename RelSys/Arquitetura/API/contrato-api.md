# Contrato da API REST do RelSys

Versão: `1.0.0` · Base: `/api/v1` · Formato: JSON (`application/json`) · PDF: `application/pdf`

Este contrato consolida as chamadas já presentes nos diagramas de sequência dos casos de uso e formaliza lacunas necessárias para autenticação, idempotência, paginação e erros. Itens marcados como **proposto** não existem hoje nos UCs, mas são necessários para atender RNF03-RNF05.

## 1. Convenções

| Item | Regra |
|---|---|
| Autenticação | `Authorization: Bearer <JWT>` obrigatório em todos os endpoints, exceto `POST /auth/login` |
| Empresa ativa | `X-Company-Id` opcional para selecionar entre empresas permitidas; o servidor valida o vínculo e nunca aceita o cabeçalho como autoridade isolada |
| Idempotência | `Idempotency-Key` obrigatório em operações de venda, cancelamento, recebimento e abertura/fechamento de caixa |
| Rastreabilidade | `X-Request-Id` opcional; se ausente, o servidor gera um e devolve no header de resposta |
| Paginação | `page` (0-based) e `size` (1..100); resposta sempre com `items`, `page`, `size`, `totalElements` |
| Valores | `number` com 2 casas decimais; moeda implícita `BRL` no MVP |
| Datas | ISO-8601 (`2026-11-26T14:30:00Z`) |
| Erros | Corpo `Problem`, conforme seção 4 |
| Auditabilidade | Toda mutação registra `userId`, `companyId`, timestamp e identificador da entidade afetada |

## 2. Matriz de endpoints

| Método | Path | Origem | Permissão sugerida |
|---|---|---|---|
| `POST` | `/auth/login` | UC14 | pública |
| `POST` | `/auth/logout` | proposto | autenticado |
| `POST` | `/auth/refresh` | proposto | refresh token |
| `GET` | `/auth/me` | proposto | autenticado |
| `GET` | `/clients` | UC02 | `CLIENT:GET` |
| `POST` | `/clients` | UC02 | `CLIENT:POST` |
| `PUT` | `/clients/{id}` | UC02 | `CLIENT:PUT` |
| `PATCH` | `/clients/{id}/status` | proposto (RF02) | `CLIENT:STATUS` |
| `PATCH` | `/clients/{id}/anonymize` | proposto (RNF03) | `CLIENT:ANONYMIZE` |
| `GET` | `/suppliers` | UC03 | `SUPPLIER:GET` |
| `POST` | `/suppliers` | UC03 | `SUPPLIER:POST` |
| `PUT` | `/suppliers/{id}` | UC03 | `SUPPLIER:PUT` |
| `PATCH` | `/suppliers/{id}/exemption` | UC03 | `SUPPLIER:EXEMPTION` |
| `GET` | `/payment-methods` | UC01 | `PAYMENT_METHOD:GET` |
| `POST` | `/payment-methods` | UC01 | `PAYMENT_METHOD:POST` |
| `PATCH` | `/payment-methods/{id}/status` | UC01 | `PAYMENT_METHOD:STATUS` |
| `GET` | `/products` | UC05 | `PRODUCT:GET` |
| `GET` | `/products/search?q=` | UC09 | `PRODUCT:GET` |
| `POST` | `/products` | UC04 | `PRODUCT:POST` |
| `POST` | `/stock/receive` | UC07 | `STOCK:RECEIVE` |
| `POST` | `/labels/print` | UC06 | `LABEL:PRINT` |
| `GET` | `/cash-sessions/active` | UC08 | `CASH_SESSION:GET` |
| `POST` | `/cash-sessions/open` | UC08 | `CASH_SESSION:OPEN` |
| `POST` | `/cash-movements` | UC08 | `CASH_MOVEMENT:POST` |
| `POST` | `/cash-sessions/{id}/close` | UC08 | `CASH_SESSION:CLOSE` |
| `PATCH` | `/cash-sessions/{id}/confirm-close` | UC08 | `CASH_SESSION:CONFIRM` |
| `POST` | `/sales/validate-discount` | UC11 | `SALE:VALIDATE_DISCOUNT` |
| `POST` | `/sales` | UC11 | `SALE:POST` |
| `GET` | `/sales/{id}/cancel-check` | UC12 | `SALE:CANCEL_CHECK` |
| `POST` | `/sales/{id}/cancel` | UC12 | `SALE:CANCEL` |
| `POST` | `/sales/{id}/receipt/reprint` | proposto (UC10) | `SALE:REPRINT` |
| `GET` | `/dashboard/summary` | UC14 | `DASHBOARD:GET` |
| `GET` | `/reports/sales` | UC13 | `REPORT:GET` |
| `GET` | `/reports/sales/export-pdf` | UC13 | `REPORT:EXPORT` |
| `GET` | `/roles` | UC15 | `ROLE:GET` |
| `POST` | `/roles` | UC15 | `ROLE:POST` |
| `PUT` | `/roles/{id}/permissions` | UC15 | `ROLE:PERMISSIONS` |
| `GET` | `/permissions` | proposto (UC15) | `PERMISSION:GET` |
| `GET` | `/users` | UC16 | `USER:GET` |
| `POST` | `/users` | UC16 | `USER:POST` |
| `PUT` | `/users/{id}` | UC16 | `USER:PUT` |
| `PATCH` | `/users/{id}/status` | UC16 | `USER:STATUS` |

## 3. Especificação OpenAPI

```yaml
openapi: 3.0.3
info:
  title: RelSys API
  version: 1.0.0
  description: Contrato REST do ERP RelSys. Empresa ativa é resolvida no servidor a partir da sessão autenticada.
servers:
  - url: https://{host}
    variables:
      host:
        default: localhost:8080
security:
  - bearerAuth: []
tags:
  - name: Auth
  - name: Clients
  - name: Suppliers
  - name: PaymentMethods
  - name: Products
  - name: Stock
  - name: Cash
  - name: Sales
  - name: Reports
  - name: Administration

paths:
  /api/v1/auth/login:
    post:
      tags: [Auth]
      summary: Autentica usuário e retorna token com empresas e permissões
      security: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LoginRequest'
      responses:
        '200':
          description: Login realizado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/LoginResponse'
        '401':
          description: Credenciais inválidas ou usuário inativo
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/auth/logout:
    post:
      tags: [Auth]
      summary: Revoga a sessão atual
      responses:
        '204':
          description: Sessão revogada

  /api/v1/auth/refresh:
    post:
      tags: [Auth]
      summary: Renova access token usando refresh token
      security: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [refreshToken]
              properties:
                refreshToken:
                  type: string
      responses:
        '200':
          description: Token renovado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/LoginResponse'
        '401':
          description: Refresh token inválido ou revogado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/auth/me:
    get:
      tags: [Auth]
      summary: Retorna usuário, empresa ativa e permissões da sessão
      responses:
        '200':
          description: Dados da sessão
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CurrentUser'

  /api/v1/clients:
    get:
      tags: [Clients]
      summary: Lista clientes da empresa ativa
      parameters:
        - $ref: '#/components/parameters/CompanyHeader'
        - $ref: '#/components/parameters/Search'
        - $ref: '#/components/parameters/Page'
        - $ref: '#/components/parameters/Size'
      responses:
        '200':
          description: Lista paginada
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ClientPage'
    post:
      tags: [Clients]
      summary: Cadastra cliente PF, PJ ou consumidor final
      parameters:
        - $ref: '#/components/parameters/CompanyHeader'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ClientInput'
      responses:
        '201':
          description: Cliente criado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Client'
        '422':
          description: Documento inválido ou duplicado na empresa
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/clients/{id}:
    put:
      tags: [Clients]
      summary: Atualiza cliente preservando histórico de vendas
      parameters:
        - $ref: '#/components/parameters/Id'
        - $ref: '#/components/parameters/CompanyHeader'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ClientInput'
      responses:
        '200':
          description: Cliente atualizado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Client'
        '404':
          description: Cliente não encontrado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/clients/{id}/status:
    patch:
      tags: [Clients]
      summary: Ativa ou inativa cliente
      parameters:
        - $ref: '#/components/parameters/Id'
        - $ref: '#/components/parameters/CompanyHeader'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [active]
              properties:
                active:
                  type: boolean
      responses:
        '200':
          description: Status alterado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Client'

  /api/v1/clients/{id}/anonymize:
    patch:
      tags: [Clients]
      summary: Anonimiza dados pessoais mantendo o histórico comercial
      parameters:
        - $ref: '#/components/parameters/Id'
        - $ref: '#/components/parameters/CompanyHeader'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [reason]
              properties:
                reason:
                  type: string
                  minLength: 5
      responses:
        '200':
          description: Cliente anonimizado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Client'
        '409':
          description: Cliente já anonimizado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/suppliers:
    get:
      tags: [Suppliers]
      summary: Lista fornecedores da empresa ativa
      parameters:
        - $ref: '#/components/parameters/CompanyHeader'
        - $ref: '#/components/parameters/Page'
        - $ref: '#/components/parameters/Size'
      responses:
        '200':
          description: Lista paginada
          content:
            application/json:
              schema:
                type: object
                properties:
                  items:
                    type: array
                    items:
                      $ref: '#/components/schemas/Supplier'
                  page:
                    type: integer
                  size:
                    type: integer
                  totalElements:
                    type: integer
    post:
      tags: [Suppliers]
      summary: Cadastra fornecedor
      parameters:
        - $ref: '#/components/parameters/CompanyHeader'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/SupplierInput'
      responses:
        '201':
          description: Fornecedor criado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Supplier'

  /api/v1/suppliers/{id}:
    put:
      tags: [Suppliers]
      summary: Atualiza fornecedor
      parameters:
        - $ref: '#/components/parameters/Id'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/SupplierInput'
      responses:
        '200':
          description: Fornecedor atualizado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Supplier'

  /api/v1/suppliers/{id}/exemption:
    patch:
      tags: [Suppliers]
      summary: Define isenção de desconto do fornecedor
      parameters:
        - $ref: '#/components/parameters/Id'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [exemptFromDiscount]
              properties:
                exemptFromDiscount:
                  type: boolean
      responses:
        '200':
          description: Isenção atualizada
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Supplier'

  /api/v1/payment-methods:
    get:
      tags: [PaymentMethods]
      summary: Lista métodos de pagamento da empresa ativa
      parameters:
        - $ref: '#/components/parameters/CompanyHeader'
      responses:
        '200':
          description: Lista de métodos
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/PaymentMethod'
    post:
      tags: [PaymentMethods]
      summary: Cadastra método de pagamento ativo
      parameters:
        - $ref: '#/components/parameters/CompanyHeader'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [name, acronym, type]
              properties:
                name:
                  type: string
                acronym:
                  type: string
                type:
                  type: string
                  enum: [SINGLE, DEFERRED]
      responses:
        '201':
          description: Método criado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PaymentMethod'

  /api/v1/payment-methods/{id}/status:
    patch:
      tags: [PaymentMethods]
      summary: Ativa ou inativa método sem excluir o registro
      parameters:
        - $ref: '#/components/parameters/Id'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [active]
              properties:
                active:
                  type: boolean
      responses:
        '200':
          description: Status alterado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PaymentMethod'

  /api/v1/products:
    get:
      tags: [Products]
      summary: Lista produtos com saldo e validade
      parameters:
        - $ref: '#/components/parameters/CompanyHeader'
        - $ref: '#/components/parameters/Search'
        - $ref: '#/components/parameters/Page'
        - $ref: '#/components/parameters/Size'
      responses:
        '200':
          description: Lista paginada
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ProductPage'
    post:
      tags: [Products]
      summary: Cadastra produto e gera código interno se omitido
      parameters:
        - $ref: '#/components/parameters/CompanyHeader'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ProductInput'
      responses:
        '201':
          description: Produto criado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Product'
        '409':
          description: Código de barras duplicado na empresa
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/products/search:
    get:
      tags: [Products]
      summary: Busca rápida usada pelo PDV
      parameters:
        - name: q
          in: query
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Produtos disponíveis para venda
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Product'

  /api/v1/stock/receive:
    post:
      tags: [Stock]
      summary: Registra recebimento e cria lote distinto
      parameters:
        - $ref: '#/components/parameters/IdempotencyKey'
        - $ref: '#/components/parameters/CompanyHeader'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/StockReceiveRequest'
      responses:
        '201':
          description: Lote criado e movimentação de entrada registrada
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/StockBatch'
        '422':
          description: Validade anterior à data atual
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/labels/print:
    post:
      tags: [Stock]
      summary: Envia fila de etiquetas para ZPL, PPLA ou PDF A4
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LabelPrintRequest'
      responses:
        '202':
          description: Impressão enfileirada
          content:
            application/json:
              schema:
                type: object
                properties:
                  jobId:
                    type: string
                  status:
                    type: string
                    enum: [QUEUED]

  /api/v1/cash-sessions/active:
    get:
      tags: [Cash]
      summary: Retorna a sessão aberta do operador ou solicita abertura
      responses:
        '200':
          description: Sessão ativa
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CashSession'
        '404':
          description: Nenhuma sessão aberta
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/cash-sessions/open:
    post:
      tags: [Cash]
      summary: Abre sessão com fundo de troco
      parameters:
        - $ref: '#/components/parameters/IdempotencyKey'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [openingAmount]
              properties:
                openingAmount:
                  type: number
      responses:
        '201':
          description: Caixa aberto
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CashSession'
        '409':
          description: Já existe sessão aberta
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/cash-movements:
    post:
      tags: [Cash]
      summary: Registra sangria, reforço ou estorno na sessão ativa
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CashMovementRequest'
      responses:
        '201':
          description: Movimentação registrada
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CashMovement'

  /api/v1/cash-sessions/{id}/close:
    post:
      tags: [Cash]
      summary: Calcula divergências por operação e meio de pagamento
      parameters:
        - $ref: '#/components/parameters/Id'
        - $ref: '#/components/parameters/IdempotencyKey'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CashCloseRequest'
      responses:
        '200':
          description: Demonstrativo pendente de confirmação
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CashCloseResult'

  /api/v1/cash-sessions/{id}/confirm-close:
    patch:
      tags: [Cash]
      summary: Confirma o fechamento da sessão
      parameters:
        - $ref: '#/components/parameters/Id'
        - $ref: '#/components/parameters/IdempotencyKey'
      responses:
        '200':
          description: Sessão fechada com ou sem divergência
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/CashSession'

  /api/v1/sales/validate-discount:
    post:
      tags: [Sales]
      summary: Valida isenção de desconto por produto e fornecedor
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/DiscountValidationRequest'
      responses:
        '200':
          description: Resultado da validação
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/DiscountValidation'
        '422':
          description: Desconto bloqueado por isenção
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/sales:
    post:
      tags: [Sales]
      summary: Finaliza venda, baixa estoque PEPS/FEFO e registra recebimento
      parameters:
        - $ref: '#/components/parameters/IdempotencyKey'
        - $ref: '#/components/parameters/CompanyHeader'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/SaleRequest'
      responses:
        '201':
          description: Venda processada
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Sale'
        '409':
          description: Caixa fechado, produto sem saldo ou método inativo
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/sales/{id}/cancel-check:
    get:
      tags: [Sales]
      summary: Verifica permissão, status da venda e sessão ativa antes do cancelamento
      parameters:
        - $ref: '#/components/parameters/Id'
      responses:
        '200':
          description: Cancelamento permitido
          content:
            application/json:
              schema:
                type: object
                properties:
                  allowed:
                    type: boolean
                  reasonCode:
                    type: string
        '403':
          description: Sem permissão
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'
        '409':
          description: Venda já cancelada
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/sales/{id}/cancel:
    post:
      tags: [Sales]
      summary: Cancela venda, devolve lotes e estorna caixa em uma transação
      parameters:
        - $ref: '#/components/parameters/Id'
        - $ref: '#/components/parameters/IdempotencyKey'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [reason]
              properties:
                reason:
                  type: string
                  minLength: 5
      responses:
        '200':
          description: Venda cancelada e estorno concluído
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Sale'
        '422':
          description: Motivo não informado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/sales/{id}/receipt/reprint:
    post:
      tags: [Sales]
      summary: Reenvia comprovante não fiscal para a térmica
      parameters:
        - $ref: '#/components/parameters/Id'
      responses:
        '202':
          description: Reimpressão enfileirada; venda permanece gravada
          content:
            application/json:
              schema:
                type: object
                properties:
                  jobId:
                    type: string
                  status:
                    type: string
                    enum: [QUEUED]

  /api/v1/dashboard/summary:
    get:
      tags: [Reports]
      summary: Retorna faturamento do dia, top produtos e alertas da empresa
      responses:
        '200':
          description: Resumo gerencial
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/DashboardSummary'

  /api/v1/reports/sales:
    get:
      tags: [Reports]
      summary: Histórico imutável de vendas com filtros
      parameters:
        - name: startDate
          in: query
          schema:
            type: string
            format: date-time
        - name: endDate
          in: query
          schema:
            type: string
            format: date-time
        - name: clientId
          in: query
          schema:
            type: integer
        - name: status
          in: query
          schema:
            type: string
            enum: [PROCESSED, CANCELED]
        - name: paymentMethodId
          in: query
          schema:
            type: integer
        - $ref: '#/components/parameters/Page'
        - $ref: '#/components/parameters/Size'
      responses:
        '200':
          description: Relatório paginado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/SalePage'

  /api/v1/reports/sales/export-pdf:
    get:
      tags: [Reports]
      summary: Exporta o relatório filtrado em PDF
      parameters:
        - name: startDate
          in: query
          schema:
            type: string
            format: date-time
        - name: endDate
          in: query
          schema:
            type: string
            format: date-time
        - name: status
          in: query
          schema:
            type: string
            enum: [PROCESSED, CANCELED]
      responses:
        '200':
          description: PDF gerado
          content:
            application/pdf:
              schema:
                type: string
                format: binary
        '404':
          description: Nenhum resultado para os filtros informados
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/permissions:
    get:
      tags: [Administration]
      summary: Lista o catálogo de permissões para a matriz de perfis
      responses:
        '200':
          description: Catálogo de permissões
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Permission'

  /api/v1/roles:
    get:
      tags: [Administration]
      summary: Lista perfis da empresa
      responses:
        '200':
          description: Perfis
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Role'
    post:
      tags: [Administration]
      summary: Cria perfil sem nenhuma permissão
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [name]
              properties:
                name:
                  type: string
      responses:
        '201':
          description: Perfil criado sem permissões
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Role'

  /api/v1/roles/{id}/permissions:
    put:
      tags: [Administration]
      summary: Substitui a matriz de permissões do perfil
      parameters:
        - $ref: '#/components/parameters/Id'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [permissionIds]
              properties:
                permissionIds:
                  type: array
                  items:
                    type: integer
      responses:
        '200':
          description: Permissões atualizadas para próximas requisições
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Role'

  /api/v1/users:
    get:
      tags: [Administration]
      summary: Lista usuários da empresa ativa
      responses:
        '200':
          description: Usuários
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
    post:
      tags: [Administration]
      summary: Cadastra usuário com senha armazenada em hash
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UserInput'
      responses:
        '201':
          description: Usuário criado ativo
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '422':
          description: Campos obrigatórios vazios ou e-mail duplicado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Problem'

  /api/v1/users/{id}:
    put:
      tags: [Administration]
      summary: Atualiza dados e perfil do usuário
      parameters:
        - $ref: '#/components/parameters/Id'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UserInput'
      responses:
        '200':
          description: Usuário atualizado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'

  /api/v1/users/{id}/status:
    patch:
      tags: [Administration]
      summary: Ativa ou inativa usuário e revoga sessões ativas
      parameters:
        - $ref: '#/components/parameters/Id'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [active]
              properties:
                active:
                  type: boolean
      responses:
        '200':
          description: Status alterado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'

components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
  parameters:
    Id:
      name: id
      in: path
      required: true
      schema:
        type: integer
        format: int64
    CompanyHeader:
      name: X-Company-Id
      in: header
      required: false
      schema:
        type: integer
        format: int64
      description: Seleção da empresa ativa entre vínculos autorizados do usuário.
    IdempotencyKey:
      name: Idempotency-Key
      in: header
      required: true
      schema:
        type: string
      description: Chave única do cliente para tornar a mutação segura contra repetição.
    Search:
      name: search
      in: query
      schema:
        type: string
    Page:
      name: page
      in: query
      schema:
        type: integer
        minimum: 0
        default: 0
    Size:
      name: size
      in: query
      schema:
        type: integer
        minimum: 1
        maximum: 100
        default: 20
  schemas:
    Problem:
      type: object
      required: [code, message, traceId]
      properties:
        code:
          type: string
          example: MSG24
        message:
          type: string
          example: Desconto Bloqueado
        traceId:
          type: string
        fieldErrors:
          type: array
          items:
            type: object
            properties:
              field:
                type: string
              message:
                type: string
    LoginRequest:
      type: object
      required: [email, password]
      properties:
        email:
          type: string
          format: email
        password:
          type: string
          format: password
        companyId:
          type: integer
          format: int64
    LoginResponse:
      type: object
      properties:
        accessToken:
          type: string
        refreshToken:
          type: string
        tokenType:
          type: string
          example: Bearer
        expiresIn:
          type: integer
          example: 900
        user:
          $ref: '#/components/schemas/CurrentUser'
    CurrentUser:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        email:
          type: string
        activeCompanyId:
          type: integer
        companies:
          type: array
          items:
            type: object
            properties:
              id:
                type: integer
              tradeName:
                type: string
              isHeadquarter:
                type: boolean
        permissions:
          type: array
          items:
            type: string
            example: SALE:CANCEL
    ClientInput:
      type: object
      required: [personType, name]
      properties:
        personType:
          type: string
          enum: [P, L, D]
          description: P física, L jurídica, D consumidor final sem documento
        name:
          type: string
        email:
          type: string
          format: email
        document:
          type: string
          description: CPF ou CNPJ conforme personType
    Client:
      type: object
      properties:
        id:
          type: integer
        personType:
          type: string
          enum: [P, L, D]
        name:
          type: string
        email:
          type: string
        document:
          type: string
        status:
          type: string
          enum: [ACTIVE, INACTIVE, ANONYMIZED]
        anonymizedAt:
          type: string
          format: date-time
    ClientPage:
      type: object
      properties:
        items:
          type: array
          items:
            $ref: '#/components/schemas/Client'
        page:
          type: integer
        size:
          type: integer
        totalElements:
          type: integer
    SupplierInput:
      type: object
      required: [name]
      properties:
        name:
          type: string
        document:
          type: string
        exemptFromDiscount:
          type: boolean
          default: false
    Supplier:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        document:
          type: string
        exemptFromDiscount:
          type: boolean
        status:
          type: string
          enum: [ACTIVE, INACTIVE]
    PaymentMethod:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        acronym:
          type: string
        type:
          type: string
          enum: [SINGLE, DEFERRED]
        active:
          type: boolean
    ProductInput:
      type: object
      required: [name, price]
      properties:
        name:
          type: string
        barcode:
          type: string
        price:
          type: number
        minimumStock:
          type: number
        supplierIds:
          type: array
          items:
            type: integer
        discountExempt:
          type: boolean
    Product:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        barcode:
          type: string
        price:
          type: number
        minimumStock:
          type: number
        availableQuantity:
          type: number
        nearestExpiration:
          type: string
          format: date-time
        active:
          type: boolean
    ProductPage:
      type: object
      properties:
        items:
          type: array
          items:
            $ref: '#/components/schemas/Product'
        page:
          type: integer
        size:
          type: integer
        totalElements:
          type: integer
    StockReceiveRequest:
      type: object
      required: [productId, quantity, purchasePrice]
      properties:
        productId:
          type: integer
        supplierId:
          type: integer
        quantity:
          type: number
          exclusiveMinimum: true
          minimum: 0
        purchasePrice:
          type: number
        expirationDate:
          type: string
          format: date-time
        warehouseId:
          type: integer
        locationId:
          type: integer
    StockBatch:
      type: object
      properties:
        id:
          type: integer
        productId:
          type: integer
        purchasePrice:
          type: number
        expirationDate:
          type: string
          format: date-time
        availableQuantity:
          type: number
    LabelPrintRequest:
      type: object
      required: [format, items]
      properties:
        format:
          type: string
          enum: [ZPL, PPLA, PDF_A4]
        copiesPerItem:
          type: integer
          default: 1
        items:
          type: array
          items:
            type: object
            properties:
              productId:
                type: integer
              quantity:
                type: integer
    CashSession:
      type: object
      properties:
        id:
          type: integer
        operatorId:
          type: integer
        status:
          type: string
          enum: [OPEN, CLOSING, CLOSED_NO_DIVERGENCE, CLOSED_WITH_DIVERGENCE]
        openedAt:
          type: string
          format: date-time
        closedAt:
          type: string
          format: date-time
        openingAmount:
          type: number
    CashMovementRequest:
      type: object
      required: [type, value, reason]
      properties:
        type:
          type: string
          enum: [WITHDRAWAL, REFILL, REVERSAL]
        value:
          type: number
        reason:
          type: string
          minLength: 3
    CashMovement:
      type: object
      properties:
        id:
          type: integer
        cashSessionId:
          type: integer
        type:
          type: string
          enum: [WITHDRAWAL, REFILL, REVERSAL]
        value:
          type: number
        reason:
          type: string
        createdAt:
          type: string
          format: date-time
    CashCloseRequest:
      type: object
      required: [countedByMethod]
      properties:
        countedByMethod:
          type: array
          items:
            type: object
            properties:
              paymentMethodId:
                type: integer
              value:
                type: number
    CashCloseResult:
      type: object
      properties:
        sessionId:
          type: integer
        expectedByMethod:
          type: array
          items:
            type: object
            properties:
              paymentMethodId:
                type: integer
              value:
                type: number
        countedByMethod:
          type: array
          items:
            type: object
            properties:
              paymentMethodId:
                type: integer
              value:
                type: number
        divergences:
          type: array
          items:
            type: object
            properties:
              operation:
                type: string
              paymentMethodId:
                type: integer
              difference:
                type: number
        confirmed:
          type: boolean
    DiscountValidationRequest:
      type: object
      required: [items]
      properties:
        discountPercentage:
          type: number
        items:
          type: array
          items:
            type: object
            properties:
              productId:
                type: integer
              supplierId:
                type: integer
              discountPercentage:
                type: number
    DiscountValidation:
      type: object
      properties:
        allowed:
          type: boolean
        blockedItems:
          type: array
          items:
            type: integer
        reasonCode:
          type: string
          example: MSG24
    SaleRequest:
      type: object
      required: [cashSessionId, paymentMethodId, items]
      properties:
        clientId:
          type: integer
          nullable: true
        cashSessionId:
          type: integer
        paymentMethodId:
          type: integer
        discountPercentage:
          type: number
        items:
          type: array
          items:
            $ref: '#/components/schemas/SaleItemInput'
    SaleItemInput:
      type: object
      required: [productId, quantity]
      properties:
        productId:
          type: integer
        quantity:
          type: number
        discountPercentage:
          type: number
    Sale:
      type: object
      properties:
        id:
          type: integer
        status:
          type: string
          enum: [PROCESSED, CANCELED]
        totalValue:
          type: number
        clientId:
          type: integer
          nullable: true
        paymentMethodId:
          type: integer
        operatorId:
          type: integer
        canceledAt:
          type: string
          format: date-time
        cancellationReason:
          type: string
        createdAt:
          type: string
          format: date-time
        items:
          type: array
          items:
            type: object
            properties:
              productId:
                type: integer
              productName:
                type: string
              productPrice:
                type: number
              quantity:
                type: number
    SalePage:
      type: object
      properties:
        items:
          type: array
          items:
            $ref: '#/components/schemas/Sale'
        page:
          type: integer
        size:
          type: integer
        totalElements:
          type: integer
    DashboardSummary:
      type: object
      properties:
        dailyRevenue:
          type: number
        topProducts:
          type: array
          items:
            type: object
            properties:
              productId:
                type: integer
              name:
                type: string
              quantity:
                type: number
        lowStock:
          type: array
          items:
            $ref: '#/components/schemas/Product'
        expiringBatches:
          type: array
          items:
            $ref: '#/components/schemas/StockBatch'
    Permission:
      type: object
      properties:
        id:
          type: integer
        entity:
          type: string
          example: SALE
        action:
          type: string
          example: CANCEL
        method:
          type: string
          enum: [GET, GET_ID, POST, PUT, PATCH, DELETE]
    Role:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        permissions:
          type: array
          items:
            $ref: '#/components/schemas/Permission'
    UserInput:
      type: object
      required: [name, email, password, roleId, companyId]
      properties:
        name:
          type: string
        email:
          type: string
          format: email
        password:
          type: string
          format: password
        roleId:
          type: integer
        companyId:
          type: integer
    User:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        email:
          type: string
        role:
          $ref: '#/components/schemas/Role'
        companyId:
          type: integer
        active:
          type: boolean
```

## 4. Erros padronizados

| HTTP | `code` | Situação típica | Mensagem |
|---|---|---|---|
| 400 | `VALIDATION_ERROR` | Parâmetro inválido | Ver `fieldErrors` |
| 401 | `AUTH_REQUIRED` | Token ausente/expirado | “Sessão expirada. Faça login novamente.” |
| 403 | `PERMISSION_DENIED` | Perfil sem permissão | MSG28 para cancelamento |
| 404 | `NOT_FOUND` | Entidade inexistente ou fora da empresa | “Registro não encontrado.” |
| 409 | `INVALID_STATE` | Venda já cancelada; caixa já aberto/fechado | MSG27 |
| 409 | `IDEMPOTENCY_CONFLICT` | Mesma chave com payload diferente | “Chave de idempotência já usada.” |
| 409 | `TENANT_MISMATCH` | Recurso de outra empresa | “Acesso negado.” |
| 422 | `BUSINESS_RULE` | Desconto isento, motivo vazio, validade passada | MSG24, MSG26 |
| 503 | `PRINT_UNAVAILABLE` | Impressora sem papel/offline | A venda permanece gravada; usar 2ª via |

> Por segurança, `404` e `409 TENANT_MISMATCH` devem ter a mesma mensagem externa para não revelar existência de registros de outras empresas.

## 5. Idempotência e transações

| Operação | Chave | Comportamento em repetição |
|---|---|---|
| `POST /stock/receive` | `Idempotency-Key` | Retorna o mesmo lote; não cria segunda entrada |
| `POST /cash-sessions/open` | `Idempotency-Key` | Retorna a sessão já aberta |
| `POST /sales` | `Idempotency-Key` | Retorna a mesma venda; não baixa estoque duas vezes |
| `POST /sales/{id}/cancel` | `Idempotency-Key` | Retorna a venda cancelada; não estorna duas vezes |
| `POST /cash-sessions/{id}/close` | `Idempotency-Key` | Recalcula/retorna o mesmo demonstrativo |

`POST /sales` é uma transação única: gravação da venda, baixa PEPS/FEFO em `PRODUCT_SALE_BATCH`, geração de título, movimento de caixa e enfileiramento do comprovante. Falha de impressão nunca faz rollback da venda.
