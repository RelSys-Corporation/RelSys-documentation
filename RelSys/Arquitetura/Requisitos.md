### Visão Geral do Sistema

O Relsys é um sistema ERP e gerenciador de estoque projetado para controle operacional de estabelecimentos comerciais. O sistema tem foco em gestão de inventário por lotes, frente de caixa (PDV), controle de fornecedores, rastreabilidade financeira e painéis gerenciais. A segurança, a auditabilidade e o controle de acesso são pilares fundamentais, garantindo que as operações sejam realizadas apenas por usuários devidamente autorizados.

### 1. Requisitos Funcionais (RF)

#### Módulo de Segurança e Acesso

- RF01 - Gerenciar Usuários, Funções e Permissões: O sistema deve permitir a parametrização de usuários, perfis de acesso e permissões. O acesso a menus, telas e ações críticas (ex: aplicar desconto, cancelar venda, registrar sangria) deve ser regulado por controle de acesso baseado em perfis.
    

#### Módulo de Cadastros e Pessoas

- RF02 - Gerenciar Clientes: O sistema deve permitir criar, ler, atualizar e anonimizar/inativar cadastros de clientes, suportando tanto Pessoas Físicas (CPF) quanto Pessoas Jurídicas (CNPJ).
    
- RF03 - Gerenciar Fornecedores: O sistema deve permitir o cadastro e manutenção de dados dos fornecedores (Pessoa Física ou Jurídica) e suas regras de isenção comercial.
    
- RF04 - Gerenciar Métodos de Pagamento: O sistema deve permitir o cadastro e a parametrização (ativação/inativação) de formas de pagamento aceitas pelo estabelecimento (ex: Dinheiro, Cartões, Pix).
    

#### Módulo de Estoque e Produtos

- RF05 - Gerenciar Produtos: O sistema deve permitir o cadastro de produtos. Um mesmo produto pode estar vinculado a múltiplos fornecedores. O usuário deve poder configurar a isenção de desconto diretamente no cadastro do produto ou por fornecedor.
    
- RF06 - Buscar por Código de Barras: O sistema deve permitir a entrada e pesquisa de produtos utilizando leitores de código de barras ou digitação.
    
- RF07 - Gerar Código de Barras: O sistema deve gerar automaticamente um código de barras interno único, caso o produto cadastrado não possua um código de fábrica.
    
- RF08 - Gerar Etiquetas: O sistema deve possuir uma interface para gerar e imprimir etiquetas contendo o nome do produto e seu código de barras.
    
- RF09 - Visualizar Estoque e Validades: O sistema deve possuir uma tela dedicada para consulta rápida da posição atual do estoque, exibindo quantidades disponíveis, lotes ativos e respectivas datas de validade.
    

#### Módulo de Compras e Entradas

- RF10 - Registrar Recebimento de Mercadorias: O sistema deve disponibilizar uma interface para registrar a entrada física de produtos no estoque, solicitando o produto, a quantidade recebida, o preço unitário de compra e a data de validade (quando aplicável).
    
- RF11 - Controle Individualizado por Lote (Custo e Validade): Cada entrada registrada no recebimento (RF10) deve ser armazenada como um lote distinto, preservando o preço de compra específico e a data de validade de cada remessa para viabilizar o cálculo de Custo da Mercadoria Vendida (RN02) e o monitoramento de vencimentos.
    

#### Módulo de Vendas e Caixa (PDV)

- RF12 - Realizar Vendas: O sistema deve possuir uma interface de caixa (PDV) para registrar a saída de produtos. O usuário deve poder vincular a venda a um cliente cadastrado ou realizar uma venda avulsa (sem cliente identificado).
    
- RF13 - Aplicação de Descontos: O sistema deve permitir a aplicação de descontos de forma geral no valor total da venda e/ou individualmente por produto adicionado ao carrinho.
    
- RF14 - Informar Forma de Pagamento: O sistema deve exigir e registrar o método de pagamento utilizado na venda (puxando da lista de métodos ativos cadastrados no RF04).
    
- RF15 - Imprimir Comprovante: O sistema deve emitir uma via (resumo) da venda para o cliente, formatada para impressoras térmicas (cupom não fiscal), com suporte à emissão de 2ª via.
    
- RF16 - Gerenciar Sessão e Fechamento de Caixa: O sistema deve permitir a abertura e o fechamento do caixa operacional, registrando o saldo inicial (fundo de troco), movimentações de sangria e reforço, e consolidando o fechamento com demonstrativo que explique eventuais divergências financeiras discriminadas por operação.
    
- RF17 - Cancelar Venda e Estornar Operação: O sistema deve permitir que usuários autorizados cancelem vendas concluídas mediante justificativa obrigatória, alterando o status da venda para Cancelada (sem exclusão física), devolvendo os itens aos lotes de origem e registrando o estorno na sessão de caixa.
    

#### Módulo de Relatórios e Gerenciamento

- RF18 - Dashboard Gerencial: O sistema deve exibir um painel inicial com métricas e indicadores de desempenho em tempo real: faturamento do dia, produtos mais vendidos, alertas de estoque baixo e alertas de lotes próximos do vencimento.
    
- RF19 - Relatórios de Vendas: O sistema deve gerar relatórios detalhados do histórico de vendas (incluindo vendas concluídas e canceladas), filtráveis por período, cliente, status e método de pagamento.
    
- RF20 - Exportação em PDF: O sistema deve permitir a exportação e download dos relatórios de vendas e listagens em formato PDF.
    

### 2. Regras de Negócio (RN)

- RN01 - Isenção de Desconto por Produto/Fornecedor: O sistema deve bloquear a aplicação de descontos (seja individual ou no total da venda) para produtos e fornecedores que estiverem marcados com a opção de "Isenção de Desconto" no cadastro.
    
- RN02 - Controle de Lotes (PEPS / FEFO): Como o sistema aceita preços de compra e datas de validade diferentes para o mesmo produto a cada entrada (RF11), a baixa de estoque e o cálculo de Custo da Mercadoria Vendida (CMV) devem seguir o critério PEPS (Primeiro a Entrar, Primeiro a Sair), priorizando lotes com data de validade mais próxima (FEFO) quando informada.
    
- RN03 - Unicidade de Código: Não podem existir dois produtos diferentes com o mesmo código de barras no sistema dentro da mesma empresa ou filial.
    
- RN04 - Resiliência Temporal e Imutabilidade: Os dados de histórico de vendas são imutáveis. Alterar dados cadastrais de produtos ou clientes não modifica vendas passadas. Vendas canceladas (RF17) nunca são excluídas do sistema, permanecendo registradas com status Cancelada, data/hora, responsável e motivo.
    
- RN05 - Princípio do Menor Privilégio: Por padrão, um novo perfil de usuário criado no sistema não deve ter acesso a nenhuma funcionalidade até que as permissões sejam explicitamente concedidas pelo administrador.
    
- RN06 - Rastreabilidade de Divergências no Caixa: No fechamento de caixa (RF16), o sistema deve comparar os valores contados informados pelo operador com o saldo esperado calculado a partir de cada evento registrado na sessão (fundo de troco, vendas por método de pagamento, sangrias, reforços e cancelamentos), apontando exatamente em qual operação ou meio de pagamento ocorreu qualquer sobra ou quebra.
    

### 3. Requisitos Não Funcionais (RNF)

- RNF01 - Integração com Hardware Térmico: O módulo de impressão de recibos (RF15) deve suportar as larguras padrão de impressoras térmicas do mercado (58mm e 80mm).
    
- RNF02 - Formato de Etiquetas: A geração de etiquetas (RF08) deve ser compatível com impressoras térmicas de etiquetas (ZPL/PPLA) e também ser capaz de gerar um layout A4 em PDF para impressão em folhas adesivas padrão (ex: Pimaco).
    
- RNF03 - Criptografia e Proteção de Dados (LGPD): O armazenamento de dados e a comunicação da aplicação devem empregar criptografia forte para proteção de senhas e na transmissão de dados na rede para resguardar informações sensíveis de clientes e dados financeiros da empresa. Em conformidade com a LGPD (Lei nº 13.709/2018), o sistema deve aplicar minimização de dados (permitindo venda avulsa sem identificação) e suportar a anonimização de dados pessoais de clientes sob solicitação, preservando a integridade do histórico comercial (RN04).
    
- RNF04 - Controle de Acesso por Perfis: A segurança do sistema deve ser estruturada sobre controle de acesso baseado em perfis de usuário, validando a sessão autenticada e as permissões de telas e ações a cada operação realizada.
    
- RNF05 - Arquitetura Multi-Filiais: O sistema deve suportar múltiplas empresas ou filiais sob a mesma infraestrutura. Os dados de clientes, produtos, estoque, vendas, sessões de caixa e relatórios devem ser logicamente isolados e filtrados com base no vínculo da empresa ou filial atual do usuário logado.
    
