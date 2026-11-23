# Riscos e Questões Abertas

## 1. Objetivo

Registrar divergências, decisões pendentes e riscos que não foram resolvidos nesta entrega. Os diagramas `classDiagram` e `erDiagram` pré-existentes **não foram alterados**; portanto, os itens abaixo permanecem como pendência formal até aprovação de mudança em branch específica.

## 2. Legenda

| Nível | Significado |
|---|---|
| Crítica | Impede aceite técnico ou inviabiliza requisito; precisa de decisão antes da implementação |
| Moderada | Pode gerar retrabalho ou ambiguidade; deve ser decidida na banca |
| Leve | Padronização ou clareza documental |

## 3. Questões abertas

| ID | Nível | Questão | Impacto | Recomendação | Responsável | Status |
|---|---|---|---|---|---|---|
| R-01 | Crítica | `PAYMENT_METHOD_TYPE` existe no DER, mas não há classe correspondente | Tabela órfã; tipo de pagamento não é modelado no domínio | Criar classe/enum `PaymentMethodType` ou remover tabela em branch de modelagem | Arquitetura | Aberta |
| R-02 | Crítica | `PRODUCT_SALE_BATCH` existe na classe e nas FKs, mas não tem bloco no `erDiagram` | Não há DDL definitivo para a ponte venda↔lote | Adicionar tabela ao DER com `QUANTITY`, `APPLY_DISCOUNT`, `PURCHASE_PRICE` | DBA/Arquitetura | Aberta |
| R-03 | Crítica | Flags de isenção não existem em `PRODUCT` nem em `SUPPLIER` | RN01 e UC11 não implementáveis como descritos | Definir se a flag é por produto, por fornecedor ou por relacionamento | Negócio | Aberta |
| R-04 | Crítica | Catálogos `MOVEMENT_TYPE`, `PERSON_STATUS`, `FINANCIAL_TITLE_STATUS` herdam `BaseEntityCompany`, mas não possuem `COMPANY_ID` | Multi-tenancy incoerente (RNF05) | Padronizar: catálogo global (`BaseEntity`) ou por empresa (`COMPANY_ID`) | Arquitetura | Aberta |
| R-05 | Crítica | Sessão de caixa (abertura, saldo inicial, valores contados, divergências) não existe no modelo | RF16/RN06 e UC08 não implementáveis | Criar `CASH_SESSION` e vincular `CASH_PROCESS`/`CASH_MOVEMENT` | Negócio + Arquitetura | Aberta |
| R-06 | Crítica | `SALE` não possui motivo, responsável e data de cancelamento | RF17/RN04 e UC12 não auditáveis | Adicionar colunas de cancelamento ou tabela de auditoria | Negócio + DBA | Aberta |
| R-07 | Crítica | Permissões modeladas apenas como `METHOD_NAME` (GET/POST/PUT) | Ações como “Cancelar Venda” não são representáveis | Estender modelo com `ACTION`/`MODULE` | Segurança | Aberta |
| R-08 | Crítica | Venda avulsa permitida no texto, mas classe exige exatamente 1 `Client` | Contradição de cardinalidade | Ajustar para `0..1` e `CLIENT_ID` anulável | Arquitetura | Aberta |
| R-09 | Crítica | Recebimento informa quantidade, mas `PRODUCT_BATCH` não a armazena e UC não informa armazém/localização | Entrada de estoque não registrável | Definir se quantidade fica no lote ou em `STOCK_MOVEMENT` | Estoque | Aberta |
| R-10 | Crítica | `USERS` não possui `EMAIL`; `ROLES` não possui `COMPANY_ID` | UC16 e UC15 contradizem o modelo | Incluir `EMAIL` e escopo por filial nos modelos | Segurança | Aberta |
| R-11 | Moderada | UC15 e UC16 ausentes do [[Diagrama-UC]] | Cobertura visual incompleta | Incluir nós e associações ao Administrador | Analista | Aberta |
| R-12 | Moderada | Relação `UC08 ..> UC09` parece invertida em relação ao pré-requisito | Leitura incorreta do fluxo de PDV | Trocar para `include` ou dependência | Analista | Aberta |
| R-13 | Moderada | Catálogo `MESSAGE`/`MESSAGE_ENTITY`/`MESSAGE_PROPERTY` não tem seed documentado | MSG01–MSG36 ficam órfãs | Publicar dicionário de mensagens | Analista | Aberta |
| R-14 | Moderada | `POST /api/v1/auth/login` e “sessão autenticada” não têm modelo | Endpoint fantasma | Documentar `AuthService`/`Session` | Backend | Aberta |
| R-15 | Moderada | Anonimização de clientes citada, sem campo/status no modelo | RNF03 parcialmente atendida | Definir `ANONYMIZED_AT` ou status | LGPD | Aberta |
| R-16 | Moderada | `INSTALLMENT` é `numeric`/`BigDecimal`, mas representa número da parcela | Tipagem inadequada | Alterar para inteiro | DBA | Aberta |
| R-17 | Moderada | `PAYED_VALUE` e `PAID_VALUE` convivem com `payedValue`/`paidValue` | Nomenclatura inconsistente | Padronizar `PAID_VALUE` | Backend | Aberta |
| R-18 | Moderada | `PRODUCT.ACTIVE` não existe, mas UC09 filtra “produto ativo” | Filtro impossível | Adicionar flag ou remover filtro | Produto | Aberta |
| R-19 | Moderada | `CASH_MOVEMENT` não registra “motivo” de sangria/reforço | FA01 do UC08 não auditável | Adicionar `REASON` | Financeiro | Aberta |
| R-20 | Moderada | Reimpressão de 2ª via não possui metadados | Rastreabilidade de impressão fraca | Registrar evento de impressão | PDV | Aberta |
| R-21 | Leve | `Supervisor` citado mas ausente em atores/catálogo de papéis | Ambiguidade de ator | Incluir na lista de atores/roles | Analista | Aberta |
| R-22 | Leve | `SaleStatus` redeclara campos em vez de herdar `BaseEntity` | Duplicação de padrão | Padronizar na próxima revisão do diagrama | Arquitetura | Aberta |
| R-23 | Leve | `MINIMUM_STOCK` não aparece nos UCs de cadastro | Campo não documentado | Incluir no UC04/UC05 ou marcar como parâmetro | Analista | Aberta |
| R-24 | Leve | Texto usa “Concluída”; DER usa `PROCESSED` | Vocabulário divergente | Documentar mapeamento | Analista | Aberta |

## 4. Riscos de entrega

| Risco | Probabilidade | Impacto | Mitigação |
|---|---|---|---|
| Banca rejeitar modelo por divergências críticas abertas | Alta | Alto | Apresentar esta lista com plano de ação e dono definido antes da avaliação |
| Implementação assumir interpretação errada de isenção de desconto | Média | Alto | Decisão formal em R-03 antes de codificar RN01 |
| Caixa ser implementado sem sessão e divergências | Alta | Alto | Resolver R-05 e R-19 antes de iniciar UC08 |
| Multi-tenancy inconsistente entre catálogos e entidades | Média | Alto | Resolver R-04 e R-10 |
| Protótipos desatualizados em relação aos UCs | Média | Médio | Revisar [[IndicePrototipos]] com o negócio |

## 5. Critério de fechamento

Uma questão só é fechada quando: (1) há decisão registrada, (2) o artefato afetado é atualizado em branch própria, (3) [[Rastreabilidade]] e [[QualidadeValidacao]] são ajustados e (4) [[ControleDocumental]] registra a nova versão.
