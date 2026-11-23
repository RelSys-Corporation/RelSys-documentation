# Contrato de API

## 1. Convenções

| Item | Valor |
|---|---|
| Base URL | `/api/v1` |
| Protocolo | HTTPS |
| Formato | `application/json` (exceto PDF e comandos de impressão) |
| Autenticação | Sessão autenticada obtida em `POST /auth/login` (modelo pendente — R-14) |
| Contexto | Filial ativa da sessão; quando necessário, cabeçalho `X-Company-Id` |
| Idioma das mensagens | Português (MSG01–MSG36) |
| Códigos | `200` OK, `201` criado, `204` sem conteúdo, `400` validação/MSG, `401` não autenticado, `403` sem permissão, `404` não encontrado, `409` conflito, `422` regra de negócio |

> Endpoints inferidos dos `sequenceDiagram` dos UCs. Nenhum endpoint foi alterado nos diagramas existentes.

## 2. Inventário de endpoints

| Método | Caminho | UC | Finalidade | Resposta principal |
|---|---|---|---|---|
| POST | `/auth/login` | UC14 | Autenticar e retornar sessão/permissões | 200 / 401 |
| GET | `/payment-methods` | UC01 | Listar métodos da filial | 200 |
| POST | `/payment-methods` | UC01 | Cadastrar método | 201 / MSG01 |
| PATCH | `/payment-methods/{id}/status` | UC01 | Ativar/desativar | 200 / MSG03 |
| GET | `/clients` | UC02 | Listar clientes da filial | 200 |
| POST | `/clients` | UC02 | Cadastrar cliente PF/PJ | 201 / MSG04 |
| PUT | `/clients/{id}` | UC02 | Editar cliente | 200 / MSG06 |
| GET | `/suppliers` | UC03 | Listar fornecedores | 200 |
| POST | `/suppliers` | UC03 | Cadastrar fornecedor | 201 / MSG07 |
| PUT | `/suppliers/{id}` | UC03 | Editar fornecedor | 200 |
| PATCH | `/suppliers/{id}/exemption` | UC03 | Alternar isenção | 200 |
| POST | `/products` | UC04 | Cadastrar produto | 201 / MSG09 |
| GET | `/products` | UC05 | Listar produtos/estoque | 200 |
| GET | `/products?search={termo}` | UC05 | Buscar por nome/código | 200 |
| POST | `/labels/print` | UC06 | Gerar etiquetas ZPL/PPLA/PDF | 200 (arquivo) |
| POST | `/stock/receive` | UC07 | Registrar recebimento e criar lote | 201 / MSG14 |
| GET | `/cash-sessions/active` | UC08 | Consultar sessão aberta | 200 / 404 |
| POST | `/cash-sessions/open` | UC08 | Abrir caixa com saldo inicial | 201 / MSG18 |
| POST | `/cash-movements` | UC08 | Registrar sangria/reforço | 201 / MSG19 |
| POST | `/cash-sessions/{id}/close` | UC08 | Calcular divergências | 200 |
| PATCH | `/cash-sessions/{id}/confirm-close` | UC08 | Confirmar fechamento | 200 / MSG20-21 |
| GET | `/products/search?q={codigo_ou_nome}` | UC09 | Buscar produto no PDV | 200 |
| POST | `/sales/validate-discount` | UC11 | Validar RN01 | 200 / MSG24 |
| POST | `/sales` | UC11 | Finalizar venda | 201 / aciona UC10 |
| GET | `/sales/{id}/cancel-check` | UC12 | Verificar permissão e status | 200 / 403 / 409 |
| POST | `/sales/{id}/cancel` | UC12 | Cancelar com motivo | 200 / MSG25 |
| GET | `/reports/sales` | UC13 | Histórico filtrável | 200 |
| GET | `/reports/sales/export-pdf` | UC13 | Exportar PDF | 200 (PDF) / MSG29 |
| GET | `/dashboard/summary` | UC14 | Indicadores gerenciais | 200 |
| GET | `/roles` | UC15 | Listar perfis | 200 |
| POST | `/roles` | UC15 | Criar perfil sem permissões | 201 |
| PUT | `/roles/{id}/permissions` | UC15 | Salvar matriz de permissões | 200 / MSG31 |
| GET | `/users` | UC16 | Listar usuários | 200 |
| POST | `/users` | UC16 | Cadastrar usuário | 201 / MSG33 |
| PUT | `/users/{id}` | UC16 | Editar usuário/perfil | 200 / MSG35 |
| PATCH | `/users/{id}/status` | UC16 | Ativar/inativar | 200 / MSG36 |

> UC10 (emitir comprovante) não possui endpoint próprio: é acionado por `POST /sales` e opera sobre a impressora local. A 2ª via é reemitida pela interface.

## 3. Esqueleto OpenAPI 3.0.3

```yaml
openapi: 3.0.3
info:
  title: RelSys API
  version: 1.0.0
  description: Contrato derivado dos casos de uso UC01-UC16.
servers:
  - url: /api/v1
paths:
  /auth/login:
    post:
      summary: Autenticar usuário (UC14)
      responses:
        "200": { description: Sessão e permissões }
        "401": { description: Credenciais inválidas ou usuário inativo }
  /payment-methods:
    get:
      summary: Listar métodos de pagamento (UC01)
      responses: { "200": { description: Lista } }
    post:
      summary: Cadastrar método (UC01)
      responses: { "201": { description: MSG01 } }
  /payment-methods/{id}/status:
    patch:
      summary: Ativar/desativar (UC01)
      responses: { "200": { description: MSG03 } }
  /clients:
    get:
      summary: Listar clientes (UC02)
      responses: { "200": { description: Lista } }
    post:
      summary: Cadastrar cliente (UC02)
      responses: { "201": { description: MSG04 }, "422": { description: MSG05 } }
  /clients/{id}:
    put:
      summary: Editar cliente (UC02)
      responses: { "200": { description: MSG06 } }
  /suppliers:
    get:
      summary: Listar fornecedores (UC03)
      responses: { "200": { description: Lista } }
    post:
      summary: Cadastrar fornecedor (UC03)
      responses: { "201": { description: MSG07 }, "422": { description: MSG08 } }
  /suppliers/{id}:
    put:
      summary: Editar fornecedor (UC03)
      responses: { "200": { description: OK } }
  /suppliers/{id}/exemption:
    patch:
      summary: Alternar isenção (UC03)
      responses: { "200": { description: Regra atualizada } }
  /products:
    get:
      summary: Listar produtos e saldos (UC05)
      responses: { "200": { description: Inventário } }
    post:
      summary: Cadastrar produto (UC04)
      responses: { "201": { description: MSG09 }, "422": { description: MSG10/MSG11 } }
  /products/search:
    get:
      summary: Buscar produto no PDV (UC09)
      parameters:
        - { name: q, in: query, required: true, schema: { type: string } }
      responses: { "200": { description: Correspondências } }
  /labels/print:
    post:
      summary: Gerar etiquetas (UC06)
      responses: { "200": { description: ZPL/PPLA ou PDF } }
  /stock/receive:
    post:
      summary: Registrar recebimento (UC07)
      responses: { "201": { description: MSG14 }, "422": { description: MSG15/MSG16 } }
  /cash-sessions/active:
    get:
      summary: Sessão aberta (UC08)
      responses: { "200": { description: Sessão }, "404": { description: Nenhuma } }
  /cash-sessions/open:
    post:
      summary: Abrir caixa (UC08)
      responses: { "201": { description: MSG18 } }
  /cash-movements:
    post:
      summary: Sangria/reforço (UC08)
      responses: { "201": { description: MSG19 } }
  /cash-sessions/{id}/close:
    post:
      summary: Calcular divergências (UC08)
      responses: { "200": { description: Demonstrativo } }
  /cash-sessions/{id}/confirm-close:
    patch:
      summary: Confirmar fechamento (UC08)
      responses: { "200": { description: MSG20 ou MSG21 } }
  /sales/validate-discount:
    post:
      summary: Validar isenção (UC11)
      responses: { "200": { description: Desconto OK }, "422": { description: MSG24 } }
  /sales:
    post:
      summary: Finalizar venda (UC11)
      responses: { "201": { description: Venda gravada; aciona UC10 } }
  /sales/{id}/cancel-check:
    get:
      summary: Pré-validação de cancelamento (UC12)
      responses: { "200": { description: Autorizado }, "403": { description: MSG28 }, "409": { description: MSG27 } }
  /sales/{id}/cancel:
    post:
      summary: Cancelar venda (UC12)
      responses: { "200": { description: MSG25 }, "422": { description: MSG26 } }
  /reports/sales:
    get:
      summary: Histórico de vendas (UC13)
      responses: { "200": { description: Lista }, "404": { description: MSG30 } }
  /reports/sales/export-pdf:
    get:
      summary: Exportar PDF (UC13)
      responses: { "200": { description: application/pdf }, "404": { description: MSG30 } }
  /dashboard/summary:
    get:
      summary: Indicadores da filial (UC14)
      responses: { "200": { description: Métricas }, "403": { description: Acesso restrito } }
  /roles:
    get:
      summary: Listar perfis (UC15)
      responses: { "200": { description: Lista } }
    post:
      summary: Criar perfil (UC15)
      responses: { "201": { description: Perfil sem permissões } }
  /roles/{id}/permissions:
    put:
      summary: Salvar permissões (UC15)
      responses: { "200": { description: MSG31 }, "422": { description: MSG32 } }
  /users:
    get:
      summary: Listar usuários (UC16)
      responses: { "200": { description: Lista } }
    post:
      summary: Cadastrar usuário (UC16)
      responses: { "201": { description: MSG33 }, "422": { description: MSG34 } }
  /users/{id}:
    put:
      summary: Editar usuário (UC16)
      responses: { "200": { description: MSG35 } }
  /users/{id}/status:
    patch:
      summary: Ativar/inativar usuário (UC16)
      responses: { "200": { description: MSG36 } }
components:
  securitySchemes:
    sessionAuth:
      type: apiKey
      in: header
      name: Authorization
  schemas:
    Error:
      type: object
      properties:
        code: { type: string }
        message: { type: string }
```

## 4. Pendências do contrato

| ID | Pendência |
|---|---|
| R-14 | Modelo de sessão/token não definido; `sessionAuth` é esquema proposto |
| R-03/R-08 | Payload de desconto depende da decisão sobre isenção e venda avulsa |
| R-05/R-19 | Payload de abertura/fechamento de caixa depende de `CASH_SESSION` e motivo de sangria/reforço |
| R-06 | Payload de cancelamento depende de motivo/responsável persistidos |
| R-10 | Login por e-mail depende de `USERS.EMAIL` |
