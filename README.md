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

# Etapa 3: Trigger
## Crie uma trigger que seja acionada APÓS a inserção de um novo pedido na tabela "Pedidos". A trigger deve calcular o valor total dos pedidos para o cliente correspondente e atualizar um campo "TotalPedidos" na tabela "Clientes" com o valor total. Teste a Trigger inserindo um novo pedido na tabela "Pedidos“.

```SQL
DELIMITER $$

CREATE TRIGGER CalculaTotalPedidos
AFTER INSERT ON gerenciamento.pedidos
FOR EACH ROW
BEGIN
    DECLARE total_pedidos DECIMAL(10, 2);

    -- Calcula o valor total dos pedidos para o cliente correspondente
    SELECT SUM(valor_pedido) INTO total_pedidos
    FROM gerenciamento.pedidos
    WHERE clientes_id_clientes = NEW.clientes_id_clientes;

    -- Atualiza o campo TotalPedidos na tabela Clientes
    UPDATE gerenciamento.clientes
    SET TotalPedidos = total_pedidos
    WHERE id_clientes = NEW.clientes_id_clientes;
END $$

DELIMITER ;
```

# Etapa 4: View
## Crie uma view chamada "PedidosClientes" que combina informações das tabelas "Clientes" e "Pedidos" usando JOIN para mostrar os detalhes dos pedidos e os nomes dos clientes.

```SQL
CREATE VIEW PedidosClientes AS
SELECT
    p.id_pedidos,
    c.nome AS nome_cliente,
    p.data_pedido,
    p.descricao,
    p.valor_pedido
FROM
    gerenciamento.pedidos p
JOIN
    gerenciamento.clientes c ON p.clientes_id_clientes = c.id_clientes;

SELECT * FROM PedidosClientes;

```

# Etapa 5: Consulta com JOIN
## Escreva uma consulta SQL que utiliza JOIN para listar os detalhes dos pedidos de cada cliente, incluindo o nome do cliente e o valor total de seus pedidos. Utilize a view "PedidosClientes" para essa consulta.

```SQL
SELECT
    pc.nome_cliente,
    pc.id_pedidos,
    pc.data_pedido,
    pc.descricao,
    pc.valor_pedido,
    SUM(pc.valor_pedido) OVER (PARTITION BY pc.nome_cliente) AS total_pedidos_cliente
FROM
    PedidosClientes pc;

```

# Pedidos
![image](https://github.com/GabrielChagasAlves/Sistema-de-Gerenciamento-de-Pedidos/assets/125607847/28824116-9a63-4bb6-91dd-3af1e177772e)

# Clientes
![image](https://github.com/GabrielChagasAlves/Sistema-de-Gerenciamento-de-Pedidos/assets/125607847/46a36e94-312b-484f-91af-a479e0c3fc8e)

