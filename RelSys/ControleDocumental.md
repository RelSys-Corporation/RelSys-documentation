# Controle Documental

## 1. Identificação do documento

| Campo | Valor |
|---|---|
| Conjunto documental | Documentação de entrega v1 — RelSys |
| Versão atual | 1.0 |
| Data | 2026-11-23 |
| Branch | `docs/entrega-v1-banca` |
| Repositório | `RelSys-documentation` |
| Formato | Markdown (Obsidian) + Mermaid (`mermaid-next`) + PNG |
| Idioma | Português (Brasil) |

## 2. Histórico de versões

| Versão | Data | Alteração | Responsável | Status |
|---|---|---|---|---|
| 0.1 | anterior à branch | Diagramas de classes e DER, requisitos e casos de uso iniciais | Equipe RelSys | Base |
| 0.2 | anterior à branch | Casos de uso UC01–UC16 e diagrama de casos de uso | Equipe RelSys | Base |
| 1.0 | 2026-11-23 | Complementação dos itens obrigatórios e condicionais; criação de diagramas de estados, componentes, implantação e integração; contrato de API; rastreabilidade; qualidade; riscos; segurança; privacidade | Equipe RelSys, com apoio de IA (ver [[UsoDeIA]]) | Em revisão |

## 3. Papéis e aprovação

| Papel | Responsabilidade | Situação |
|---|---|---|
| Autor / mantenedor | Redigir e manter as notas | Equipe RelSys |
| Revisor de negócio | Validar RF/RN e casos de uso | **Pendente** |
| Revisor técnico | Validar arquitetura, API, modelos e segurança | **Pendente** |
| Aprovador final | Aceitar a entrega para a banca | **Pendente** |

> Não realizar `merge` na `main` antes do preenchimento dos campos pendentes e da resolução formal dos itens em [[RiscosQuestoesAbertas]] classificados como Crítica.

## 4. Convenções

- Nomes de arquivos: `UCxx-Nome.md`, `DER-<dominio>.md`, `classDiagram-<dominio>.md`.
- Diagramas Mermaid usam o identificador de linguagem `mermaid-next`, compatível com o plugin homônimo do Obsidian deste vault.
- Tabelas do DER usam `snake_case` maiúsculo; classes usam `PascalCase`; atributos de classe usam `camelCase`.
- Referências internas usam wikilinks `[[Nota]]`.
- Cada requisito `RFxx`, regra `RNxx` e caso `UCxx` deve aparecer em [[Rastreabilidade]].

## 5. Controle de mudança

1. Toda alteração em diagramas-base deve ser feita em branch dedicada e registrada aqui.
2. Toda divergência detectada entre texto e diagrama deve virar item em [[RiscosQuestoesAbertas]].
3. Toda nova funcionalidade deve atualizar, no mínimo: requisitos, caso de uso, contrato de API, rastreabilidade e qualidade.
4. A versão é incrementada quando houver mudança de escopo, modelo de dados ou contrato de API.
