# Uso de Inteligência Artificial

## 1. Declaração

Esta documentação foi produzida com apoio de inteligência artificial generativa para leitura, cruzamento e redação de artefatos. A ferramenta utilizada foi o assistente `opencode` (modelo `opencode/big-pickle`), operando sobre o vault local do Obsidian.

## 2. Onde a IA foi utilizada

| Atividade | Entrada | Saída |
|---|---|---|
| Auditoria de consistência | 33 notas `.md` do diretório `RelSys/` | Relatório com 35 divergências (18 críticas, 11 moderadas, 6 leves) |
| Cobertura de entrega | Lista de itens obrigatórios e condicionais informada pelo usuário | Matriz de cobertura e lacunas |
| Complementação documental | Escopo, requisitos, UCs e diagramas existentes | Notas de identificação, escopo, controle, referências, qualidade, rastreabilidade, riscos, segurança, privacidade e arquitetura |
| Diagramas adicionais | Modelos existentes e regras de negócio | Diagramas de estados, componentes, implantação e integração |
| Contrato de API | Endpoints inferidos dos `sequenceDiagram` dos UCs | [[ContratoAPI]] |

## 3. Limites e uso responsável

- A IA **não alterou** os diagramas `classDiagram` e `erDiagram` pré-existentes.
- A IA **não executou** a aplicação, testes automatizados ou validação de deployment; todo conteúdo é documental.
- Endpoints, cardinalidades e regras inferidas precisam de confirmação técnica antes de virarem implementação.
- Os itens classificados como Crítica em [[RiscosQuestoesAbertas]] exigem decisão humana e revisão dos modelos oficiais.
- Nenhum dado real de cliente, CPF/CNPJ ou credencial foi inserido pela IA; todos os exemplos são fictícios.

## 4. Revisão humana recomendada

| Revisor | Documentos a revisar | Foco |
|---|---|---|
| Negócio | [[Escopo]], [[Requisitos]], [[Rastreabilidade]] | Fidelidade das regras RN01–RN06 e do escopo |
| Arquitetura | [[Arquitetura]], [[DiagramaComponentes]], [[DiagramaImplantacao]] | Viabilidade da stack e topologia |
| Backend/API | [[ContratoAPI]], [[DiagramaIntegracao]] | Nomenclatura, códigos de status, payloads |
| Segurança/LGPD | [[SegurancaPermissoes]], [[Privacidade]] | Perfis, escopo por filial, dados pessoais |
| Banca | [[00-Identificacao]] | Completude da entrega |

## 5. Registro de prompts (resumo)

- “Auditar consistência entre classDiagram, erDiagram e documentação textual do RelSys, gerando relatório por gravidade.”
- “Verificar cobertura dos itens obrigatórios e condicionais de entrega de documentação.”
