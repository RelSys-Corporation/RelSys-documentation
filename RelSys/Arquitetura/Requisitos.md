**

# Documento de Requisitos: Projeto Relsys

Visão Geral do Sistema: O Relsys é um sistema ERP e gerenciador de estoque projetado para controle operacional de estabelecimentos comerciais. O sistema tem foco em gestão de inventário, frente de caixa (PDV), controle de fornecedores e painéis gerenciais. A segurança e o controle de acesso são pilares fundamentais, garantindo que as operações sejam realizadas apenas por usuários devidamente autorizados.

### 1. Requisitos Funcionais (RF)

Descrevem o que o sistema deve fazer e quais funcionalidades estarão disponíveis para o usuário.

Módulo de Segurança e Acesso

- RF01 - Gerenciar Usuários, Funções e Permissões: O sistema deve permitir a parametrização de usuários, cargos (roles) e permissões. O acesso a menus, telas e ações (ex: aplicar desconto, excluir venda) deve ser regulado via RBAC (Role-Based Access Control).
    

Módulo de Cadastros e Pessoas

- RF02 - Gerenciar Clientes: O sistema deve permitir criar, ler, atualizar e excluir (CRUD) cadastros de clientes, suportando tanto Pessoas Físicas (CPF) quanto Pessoas Jurídicas (CNPJ).
    
- RF03 - Gerenciar Fornecedores: O sistema deve permitir o cadastro e manutenção de dados dos fornecedores (Pessoa Física ou Jurídica).
    
- RF04 - Gerenciar Métodos de Pagamento: O sistema deve permitir o cadastro e a parametrização de formas de pagamento aceitas pelo estabelecimento (ex: Dinheiro, Cartões, Pix, etc.).
    

Módulo de Estoque e Produtos

- RF05 - Gerenciar Produtos: O sistema deve permitir o cadastro de produtos. Um mesmo produto pode estar vinculado a múltiplos fornecedores. O usuário deve poder configurar a isenção de desconto diretamente no cadastro do produto por fornecedor.
    
- RF06 - Buscar por Código de Barras: O sistema deve permitir a entrada e pesquisa de produtos utilizando leitores de código de barras.
    
- RF07 - Gerar Código de Barras: O sistema deve gerar automaticamente um código de barras interno, caso o produto cadastrado não possua um código de fábrica.
    
- RF08 - Gerar Etiquetas: O sistema deve possuir uma interface para gerar e imprimir etiquetas contendo o nome do produto, seu código de barras e o valor do produto.
    
- RF09 - Visualizar Estoque: O sistema deve possuir uma tela dedicada para consulta rápida da posição atual do estoque (quantidades disponíveis, lotes, etc.).
    

Módulo de Compras e Entradas

- RF10 - Registrar Recebimento de Mercadorias: O sistema deve registrar a entrada de produtos no estoque.
    
- RF11 - Histórico de Preço de Compra por Lote: O sistema deve permitir que o usuário informe um preço de compra específico no momento do recebimento. Cada entrada de estoque deve ser registrada como um lote distinto para controle de custos variados do mesmo produto.
    

Módulo de Vendas (PDV)

- RF12 - Realizar Vendas: O sistema deve possuir uma interface de caixa (PDV) para registrar a saída de produtos. O usuário deve poder vincular a venda a um cliente cadastrado ou realizar uma venda avulsa (sem cliente identificado).
    
- RF13 - Aplicação de Descontos: O sistema deve permitir a aplicação de descontos de forma geral no valor total da venda e/ou individualmente por produto adicionado ao carrinho.
    
- RF14 - Informar Forma de Pagamento: O sistema deve exigir e registrar o método de pagamento utilizado na venda (puxando da lista de métodos cadastrados no RF04).
    
- RF15 - Imprimir Comprovante: O sistema deve emitir uma via (resumo) da venda para o cliente, formatada para impressoras térmicas (cupom não fiscal).
    

Módulo de Relatórios e Gerenciamento

- RF16 - Dashboard Gerencial: O sistema deve exibir um painel inicial com métricas e indicadores de desempenho (ex: faturamento do dia, produtos mais vendidos, alertas de estoque baixo).
    
- RF17 - Relatórios de Vendas: O sistema deve gerar relatórios detalhados do histórico de vendas, filtráveis por período, cliente, etc.
    
- RF18 - Exportação em PDF: O sistema deve permitir a exportação e download dos relatórios de vendas e listagens em formato PDF.
    

### 2. Regras de Negócio (RN)

Condições, restrições e lógicas específicas que o sistema deve respeitar.

- RN01 - Isenção de Desconto por Produto/Fornecedor: O sistema deve bloquear a aplicação de descontos (seja individual ou no total da venda) para produtos e fornecedores que estiverem marcados com a flag de "Isenção de Desconto" no cadastro.
    
- RN02 - Controle de Lotes (PEPS): Como o sistema aceita preços de compra diferentes para o mesmo produto a cada entrada (RF11), o cálculo de custo da mercadoria vendida (CMV) e lucro deve basear-se no controle PEPS (Primeiro a Entrar, Primeiro a Sair).
    
- RN03 - Unicidade de Código: Não podem existir dois produtos diferentes com o mesmo código de barras no banco de dados.
    
- RN04 - Resiliência Temporal: Os dados de histórico de vendas são imutáveis. Alterar o nome, o preço, a categoria ou o fornecedor de um produto no cadastro (ou os dados de um cliente) não deve alterar as informações registradas em vendas que já ocorreram no passado.
    
- RN05 - Princípio do Menor Privilégio: Por padrão, um novo perfil de usuário criado no sistema não deve ter acesso a nenhuma funcionalidade até que as permissões sejam explicitamente concedidas pelo administrador.
    

### 3. Requisitos Não Funcionais (RNF)

Aspectos técnicos de arquitetura, desempenho, segurança e integração.

- RNF01 - Integração com Hardware Térmico: O módulo de impressão de recibos (RF15) deve suportar as larguras padrão de impressoras térmicas ESC/POS do mercado (58mm e 80mm).
    
- RNF02 - Formato de Etiquetas: A geração de etiquetas (RF08) deve ser compatível com impressoras térmicas de etiquetas (ZPL/PPLA) e também ser capaz de gerar um layout A4 em PDF para impressão em folhas adesivas padrão (ex: Pimaco).
    
- RNF03 - Criptografia de Dados Sensíveis: O banco de dados e a comunicação da aplicação devem empregar criptografia forte para armazenamento de senhas e na transmissão de dados na rede (HTTPS/TLS) para proteger informações sensíveis de clientes e dados financeiros da empresa.
    
- RNF04 - Controle de Acesso (RBAC): A arquitetura do sistema deve ser construída sobre o modelo RBAC (Role-Based Access Control), validando tokens de sessão (ex: JWT) e permissões de rotas/ações no backend e no frontend a cada requisição.
    

  
  
  
**