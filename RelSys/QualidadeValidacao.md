# Qualidade e Validação

## 1. Objetivo

Definir como a entrega v1 será verificada e validada, garantindo que requisitos, regras de negócio, casos de uso, modelos e contrato de API estejam coerentes e testáveis.

## 2. Estratégia de testes

| Nível | Abrangência | Evidência esperada |
|---|---|---|
| Unitário | Regras de cálculo de desconto, PEPS/FEFO, validação de CPF/CNPJ, geração de código de barras, status de títulos | Testes automatizados por módulo (`auth`, `crm`, `stock`, `commercial`, `financial`, `system`) |
| Integração | Persistência PostgreSQL, transações venda → estoque → título → caixa | Testes de integração com banco de teste e rollback |
| API/Contrato | Endpoints, payloads, códigos de status, autenticação e isolamento por filial | Contrato validado contra [[ContratoAPI]] |
| UI/Protótipo | Telas, fluxos e mensagens MSG01–MSG36 | Comparação com [[IndicePrototipos]] e UCs |
| Regressão | Histórico imutável, cancelamento, 2ª via, reimpressão | Suíte de regressão executada a cada entrega |
| Segurança | RBAC, menor privilégio, multi-tenancy, hash de senha | Cenários de [[SegurancaPermissoes]] e [[Privacidade]] |
| Aceite (UAT) | Critérios abaixo, conduzidos pelo negócio | Assinatura ou registro em [[ControleDocumental]] |

## 3. Critérios de aceite por módulo

| Módulo | Critério de aceite | Origem |
|---|---|---|
| Segurança | Novo perfil sem permissão não consegue acessar menu; usuário inativo não loga; filial A não vê dados da filial B | RF01, RN05, RNF04, RNF05 |
| Cadastros | CPF/CNPJ inválido ou duplicado na filial é bloqueado; edição não altera vendas passadas | RF02, RF03, UC02, UC03, RN04 |
| Métodos de pagamento | Cadastro, listagem e ativação/inativação persistem; método inativo não aparece no PDV | RF04, UC01 |
| Produtos e estoque | Código de barras único por empresa; lote preserva preço e validade; consulta mostra saldo e validade | RF05–RF11, RN02, RN03 |
| Vendas | Venda avulsa permitida; desconto respeita isenção; baixa PEPS/FEFO; comprovante e 2ª via disponíveis | RF12–RF15, UC09–UC11 |
| Caixa | Abertura, sangria, reforço, fechamento e divergências por operação registrados | RF16, RN06, UC08 |
| Cancelamento | Status muda para Cancelada sem exclusão; motivo e responsável preservados; estoque e caixa estornados | RF17, RN04, UC12 |
| Relatórios | Filtros por período, cliente, status e método; exportação PDF | RF19, RF20, UC13 |
| Dashboard | Indicadores da filial logada; operador sem privilégio é redirecionado | RF18, UC14, RN05 |

## 4. Validação documental para a banca

| Item | Verificação | Situação nesta entrega |
|---|---|---|
| Identificação e controle | Versão, data, branch, responsáveis e histórico | OK — [[00-Identificacao]], [[ControleDocumental]] |
| Escopo | Incluído, fora do escopo, premissas e critérios de saída | OK — [[Escopo]] |
| Atores e stakeholders | Atores, papéis e partes interessadas | OK — [[Atores]] |
| Requisitos/RN/RNF | Numeração estável e rastreável | OK — [[Requisitos]], [[Rastreabilidade]] |
| Casos de uso e sequência | 16 UCs com fluxo principal, alternativo e Mensagens | OK, com ressalva UC15/UC16 ausentes no diagrama (ver [[RiscosQuestoesAbertas]]) |
| Protótipos | Telas indexadas e associadas a UCs | OK — [[IndicePrototipos]] |
| DER e classes | Diagramas-base presentes | OK — 6 DER + 7 classDiagram, **inalterados nesta branch** |
| Diagramas condicionais | Estados, componentes, implantação e integração | OK — `Arquitetura/Diagramas/` |
| Contrato de API | Endpoints, payloads e respostas | OK — [[ContratoAPI]] |
| Qualidade, rastreabilidade, riscos, IA, referências | Documentos próprios | OK |
| Consistência texto ↔ diagrama | Divergências registradas | **Pendente** — itens críticos em [[RiscosQuestoesAbertas]] precisam de decisão |

## 5. Definition of Done (DoD)

Uma história/caso de uso só é considerado pronto quando:

1. RF/RN relacionados estão em [[Rastreabilidade]].
2. Fluxo principal e alternativos estão no UC, com mensagens do sistema.
3. Endpoint correspondente está em [[ContratoAPI]].
4. Regras de isolamento por filial e permissões estão em [[SegurancaPermissoes]].
5. Dados pessoais envolvidos estão em [[Privacidade]].
6. Testes unitários, API e UI passaram; evidência anexada ao controle de versão.
7. Nenhuma divergência nova foi adicionada a [[RiscosQuestoesAbertas]] sem responsável.

## 6. Métricas sugeridas

| Métrica | Meta |
|---|---|
| Cobertura RF → UC | 100% (RF01–RF20) |
| Cobertura RN → teste | 100% (RN01–RN06) |
| Endpoints documentados vs. implementados | 100% |
| Divergências críticas abertas na banca | 0 aprovadas para release; se restarem, constar com plano de ação |
| Casos de uso com diagrama de sequência | 16/16 |
