
| Ator                      | Descrição do ator                                                                                                                                  |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Assistente Administrativo | Assistente Administrativo: Focado na operação de bastidores (Estoque, Compras, Cadastros).                                                         |
| Operador                  | Operador de Caixa (PDV): Focado no atendimento ao cliente, vendas e recebimentos.                                                                  |
| Administrador             | Administrador / Gerente: Possui acesso total ao sistema, focado em regras de negócio, parametrizações, visão estratégica (Dashboard) e relatórios. |

## Stakeholders

| Stakeholder | Interesse no RelSys | Papel na entrega |
|---|---|---|
| Empresa contratante / gestão | Confiabilidade das vendas, estoque e relatórios | Aprovadora do escopo e dos relatórios gerenciais |
| Administradores do sistema | Parametrizar perfis, usuários, métodos de pagamento e catálogos |Validadores de RF01, UC15 e UC16 |
| Operadores de caixa | Registrar vendas, pagamentos, comprovantes e movimentações de caixa | Validadores de UC08–UC12 |
| Assistência administrativa | Manter cadastros, produtos, etiquetas e recebimentos | Validadora de UC02–UC07 |
| Contador / financeiro | Conferir títulos, fechamento de caixa e divergências | Interessado em RN06 e `FINANCIAL_TITLE` |
| Equipe de desenvolvimento | Implementar API, frontend, modelos e testes | Responsável por [[ContratoAPI]], [[QualidadeValidacao]] e [[Rastreabilidade]] |
| Banca avaliadora | Verificar completude e coerência da documentação | Destinatária de [[00-Identificacao]] |
| Titulares dos dados (clientes) | Privacidade de CPF/CNPJ e histórico comercial | Protegidos por [[Privacidade]] |

## Nota sobre o ator “Supervisor”

O UC08 (FA01) menciona “supervisor autorizado” para sangria/reforço e o UC15 cita o perfil “Supervisor de Loja”. Como esse papel ainda não está no catálogo `ROLES` nem no [[Diagrama-UC]], ele está registrado como pendência R-21 em [[RiscosQuestoesAbertas]] e não foi incluído como ator oficial nesta versão.
