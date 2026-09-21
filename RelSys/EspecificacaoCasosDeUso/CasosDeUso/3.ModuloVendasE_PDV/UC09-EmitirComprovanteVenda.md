**

Especificação do Caso de Uso: UC09 - Emitir Comprovante de Venda Fluxo Principal:

1. Imediatamente após a conclusão do UC08, o sistema compila os dados da venda (ID da venda, data/hora, itens comprados, subtotal, descontos e total pago).
    
2. O sistema formata esses dados em um layout de cupom não fiscal, ajustando as quebras de linha para bobinas de 58mm ou 80mm (RNF01).
    
3. O sistema abre a janela de impressão do navegador ou envia o comando direto para a impressora térmica padrão configurada.
    
4. O sistema exibe na tela do PDV a mensagem: "Venda finalizada com sucesso!" e um botão para "Imprimir 2ª Via" caso necessário.
    
5. O PDV é resetado, voltando para um carrinho vazio pronto para a próxima venda.
    

Fluxos Alternativos:

- FA01 - Falha na Impressora / Cancelar Impressão: Se a impressora estiver desligada ou o usuário cancelar a janela de impressão, a venda não é desfeita (pois já foi gravada no banco). O sistema permite que o usuário clique em "Imprimir 2ª Via" quando a impressora voltar a funcionar.
    

**