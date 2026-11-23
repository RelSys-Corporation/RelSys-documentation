# Política de Privacidade e Proteção de Dados

## 1. Objetivo

Estabelecer o tratamento de dados pessoais no RelSys em conformidade com a LGPD (Lei nº 13.709/2018), atendendo RNF03 e complementando [[SegurancaPermissoes]].

## 2. Inventário de dados

| Dado | Onde aparece | Finalidade | Sensibilidade |
|---|---|---|---|
| Nome de pessoas físicas/jurídicas | `PERSON.NAME`, `COMPANY.TRADE_NAME` | Identificação de clientes/fornecedores | Média |
| CPF | `PHYSICAL_PERSON.CPF`, value object `CPF` | Identificação e validação de pessoa física | Alta |
| CNPJ | `LEGAL_PERSON.CNPJ`, value object `CNPJ` | Identificação de pessoa jurídica | Média |
| E-mail | `PERSON.EMAIL`; e-mail de acesso previsto no UC16 | Contato e autenticação | Média |
| Senha | `USERS.PASSWORD` (deve ser hash) | Autenticação | Alta |
| Histórico de compras | `SALE`, `PRODUCT_SALE`, `PERSON` via `CLIENT` | Imutabilidade comercial, relatórios e auditoria | Média |
| Dados financeiros | `FINANCIAL_TITLE`, `CASH_PROCESS` | Recebíveis, baixas e fechamento de caixa | Alta |
| Dados de auditoria | `CREATED_AT`, `UPDATED_AT`, logs de caixa | Rastreabilidade | Média |

## 3. Bases legais e finalidades

| Finalidade | Base legal (LGPD) | Observação |
|---|---|---|
| Cadastro e atendimento de clientes | Art. 7º, I (consentimento) ou V (execução de contrato), conforme relação com o titular | Definir base definitiva com o jurídico |
| Cumprimento de obrigações comerciais e fiscais | Art. 7º, II (obrigação legal) | Histórico de vendas imutável (RN04) |
| Proteção de crédito e prevenção à fraude | Art. 7º, X | Uso restrito a dados necessários |
| Segurança da aplicação | Art. 7º, VI (interesse legítimo) | Logs e RBAC |

## 4. Princípios aplicados

- **Minimização:** venda avulsa não exige identificação de cliente (RF12, UC11 FA02).
- **Finalidade:** CPF/CNPJ somente para identificação e validação documental.
- **Segurança:** criptografia em trânsito (HTTPS) e em repouso para dados sensíveis; senha apenas como hash (RNF03).
- **Imutabilidade:** histórico comercial preservado (RN04); anonimização não pode destruir integridade das vendas.
- **Isolamento:** dados por filial conforme RNF05 e [[SegurancaPermissoes]].

## 5. Retenção e anonimização

| Categoria | Regra proposta | Situação |
|---|---|---|
| Dados cadastrais de cliente | Enquanto houver relação comercial ou obrigação legal | A definir pelo jurídico |
| Histórico de vendas | Preservado conforme RN04 | Modelo atual não exclui vendas |
| Títulos financeiros | Enquanto houver obrigação pendente e prazo legal | A definir |
| Logs de acesso/caixa | Prazo de auditoria interno | A definir |
| Anonimização | Substituir/ocultar CPF, e-mail e nome, mantendo vínculo histórico | **Lacuna R-15**: não há campo/status no modelo |

## 6. Direitos do titular

O RelSys deve suportar, quando aplicável: confirmação de existência, acesso, correção, anonimização, bloqueio, eliminação de dados desnecessários e informação sobre compartilhamento. A operação de anonimização depende da decisão registrada em R-15.

## 7. Compartilhamento

Não há compartilhamento externo de dados pessoais na v1. Integrações locais com impressora térmica e geração de PDF não enviam dados para terceiros — ver [[DiagramaIntegracao]].

## 8. Lacunas e responsáveis

| ID | Lacuna | Responsável |
|---|---|---|
| R-15 | Anonimização sem campo/status no modelo de dados | Negócio + Jurídico + DBA |
| R-10 | `USERS.EMAIL` ausente, apesar do login por e-mail no UC16 | Segurança |
| R-14 | Sessão autenticada/token sem modelo | Backend |
| — | Definição formal dos prazos de retenção | Jurídico |
| — | Registro de consentimento, quando aplicável | Jurídico + Produto |
