# Identificação do Projeto — RelSys

| Campo | Valor |
|---|---|
| Nome do projeto | **RelSys** |
| Natureza | Sistema ERP de gestão comercial, estoque por lotes e frente de caixa (PDV) |
| Versão da documentação | **1.0 — entrega para primeira banca** |
| Data da entrega | 2026-11-23 |
| Status | Em revisão para avaliação |
| Branch de trabalho | `docs/entrega-v1-banca` |
| Stack declarada | Angular (frontend), Quarkus (backend), PostgreSQL (banco) |
| Público-alvo da documentação | Banca avaliadora, equipe de desenvolvimento, analistas de negócio |
| Responsável pela documentação | Equipe RelSys — nomes e aprovação a confirmar em [[ControleDocumental]] |
| Idioma | Português (Brasil) |
| Formato | Notas Markdown em vault Obsidian, com diagramas Mermaid via plugin `mermaid-next` |

## Visão geral resumida

O RelSys centraliza cadastro de pessoas (físicas e jurídicas), produtos e fornecedores; controla estoque por lote com custo e validade; registra vendas com descontos, formas de pagamento e baixa automática de estoque; mantém histórico imutável, controle de caixa e relatórios gerenciais, com isolamento por empresa/filial e acesso baseado em perfis. A descrição completa está em [[Apresentacao]] e em `Arquitetura/Requisitos.md` ([[Requisitos]]).

## Escopo resumido

A entrega cobre seis módulos: Segurança e Acesso; Cadastros e Pessoas; Estoque e Produtos; Compras e Entradas; Vendas e Caixa (PDV); Relatórios e Gerenciamento. Os limites explícitos, premissas e itens fora do escopo estão em [[Escopo]].

## Índice da entrega

### Itens obrigatórios

| Item | Documento |
|---|---|
| Identificação do projeto | Este arquivo |
| Visão geral | [[Apresentacao]], `Arquitetura/Requisitos.md` — “Visão Geral do Sistema” |
| Escopo | [[Escopo]] |
| Atores e stakeholders | [[Atores]] |
| Requisitos | [[Requisitos]] |
| Regras de negócio | [[Requisitos]] — seção “Regras de Negócio (RN)” |
| Casos de uso | [[Diagrama-UC]] e notas `UC01`–`UC16` |
| Protótipos | [[IndicePrototipos]] |
| DER | [[DER-auth]], [[DER-commercial]], [[DER-crm]], [[DER-financial]], [[DER-stock]], [[DER-system]] |
| Diagrama de classes | [[classDiagram-auth]], [[classDiagram-commercial]], [[classDiagram-crm]], [[classDiagram-financial]], [[classDiagram-shared]], [[classDiagram-stock]], [[classDiagram-system]] |
| Arquitetura | [[Arquitetura]] |
| Diagramas de sequência | Seção “Diagrama de Sequência” de cada `UC01`–`UC16` |
| Qualidade e validação | [[QualidadeValidacao]] |
| Rastreabilidade | [[Rastreabilidade]] |
| Riscos e questões abertas | [[RiscosQuestoesAbertas]] |
| Uso de inteligência artificial | [[UsoDeIA]] |
| Controle documental | [[ControleDocumental]] |
| Referências | [[Referencias]] |

### Itens condicionais atendidos

| Item | Documento |
|---|---|
| Diagrama de estados | [[DiagramaEstados]] |
| Diagrama de componentes | [[DiagramaComponentes]] |
| Diagrama de implantação | [[DiagramaImplantacao]] |
| Diagrama de integração | [[DiagramaIntegracao]] |
| Contrato de API | [[ContratoAPI]] |
| Modelo de segurança e permissões | [[SegurancaPermissoes]] |
| Política de privacidade | [[Privacidade]] |

## Nota de integridade

Os diagramas `classDiagram` e `erDiagram` pré-existentes **não foram alterados nesta branch**. Divergências detectadas na auditoria e que exigem aprovação para correção dos modelos estão registradas em [[RiscosQuestoesAbertas]] e não foram silenciosamente reescritas.
