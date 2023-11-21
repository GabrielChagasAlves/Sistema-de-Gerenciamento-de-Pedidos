# Sistema de Gerenciamento de Pedidos
Sistema para armazenar e gerenciar pedidos de uma empresa
# Etapa 1: Criação de Tabelas e Inserção de Dados
## Crie as tabelas "Clientes" e "Pedidos" com campos apropriados. Insira dados de exemplo nas tabelas para simular clientes e pedidos. Certifique-se de incluir uma chave primária em cada tabela.
![image](https://github.com/GabrielChagasAlves/Sistema-de-Gerenciamento-de-Pedidos/assets/125607847/f2264930-08b1-4138-bdb5-d1e1d55c5989)

# Etapa 2: Criação de Stored Procedure
## Crie uma stored procedure chamada "InserirPedido" que permite inserir um novo pedido na tabela "Pedidos" com as informações apropriadas. A stored procedure deve receber parâmetros como o ID do cliente e os detalhes do pedido. Ao término teste o funcionamento da stored procedure criada inserindo um pedido.

```SQL
DELIMITER $$

CREATE PROCEDURE InserirPedido(
    IN cliente_id INT,
    IN nome_cliente VARCHAR(255),
    IN data_pedido DATE,
    IN descricao_pedido VARCHAR(255),
    IN valor_pedido DECIMAL(10, 2)
)
BEGIN
    -- Insira o novo pedido na tabela Pedidos
    INSERT INTO gerenciamento.pedidos (nome_clientes, data_pedido, descricao, valor_pedido, clientes_id_clientes)
    VALUES (nome_cliente, data_pedido, descricao_pedido, valor_pedido, cliente_id);
END $$

DELIMITER ;
```
