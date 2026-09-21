**

Especificação do Caso de Uso: UC08 - Registrar Pagamento e Finalizar Venda Fluxo Principal:

1. O utilizador clica em "Ir para Pagamento" na tela do PDV.
    
2. O sistema exibe o resumo dos valores.
    
3. (Opcional) O utilizador seleciona um Cliente cadastrado.
    
4. O utilizador seleciona o Método de Pagamento (ex: Pix, Cartão).
    
5. O utilizador clica em "Confirmar Pagamento".
    
6. O sistema deduz os produtos do estoque (obedecendo rigorosamente à regra de controle de lotes PEPS - RN02, abatendo primeiro os lotes mais antigos) e salva a venda no banco de dados como concluída.
    
7. O sistema chama automaticamente o caso de uso de emissão de comprovante (UC09).
    

Fluxos Alternativos:

- FA01 - Aplicar Desconto: Antes de confirmar o pagamento, o utilizador insere um valor de desconto. O sistema valida se há itens com "Isenção de Desconto" (RN01) e recalcula o total a pagar, não aplicando o desconto sobre os itens restritos.
    
- FA02 - Venda Avulsa: O utilizador ignora a seleção de cliente e finaliza a compra direto para um "Consumidor Final".
    

**