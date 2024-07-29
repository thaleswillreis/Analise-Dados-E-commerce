# Script SQL de teste

### Arquivo: `Script-5.sql`

Este arquivo contém o código de script SQL gerado no DBeaver, e que foi utilizado para gerar e testar as consultas SQL antes de serem inseridas e utilizadas dentro do código Python.

#### Código SQL:

```SQL

-- consulta SQL para contagem de pedidos por dia
SELECT
    DATE(order_purchase_timestamp) AS dia,
    COUNT(*) AS count_pedidos
FROM orders
GROUP BY dia
LIMIT 100;


-- 	consulta SQL para contagem de pedidos por dia e hora (utilizar na a subconsulta abaixo)
SELECT
    CASE STRFTIME('%w', order_purchase_timestamp)
        WHEN '1' THEN 'Seg'
        WHEN '2' THEN 'Ter'
        WHEN '3' THEN 'Qua'
        WHEN '4' THEN 'Qui'
        WHEN '5' THEN 'Sex'
        WHEN '6' THEN 'Sab'
        WHEN '0' THEN 'Dom'
        END AS dia_da_semana,
    CAST(STRFTIME('%w', order_purchase_timestamp) AS INTEGER) AS dia_da_semana_int,
    CAST(STRFTIME("%H", order_purchase_timestamp) AS INTEGER) AS hora
FROM orders LIMIT 100


-- contagem total de pedidos por dia da semana e horário
SELECT 
    dia_da_semana,
    COUNT(CASE WHEN hora = 0 THEN 1 END) AS "0"
    COUNT(CASE WHEN hora = 1 THEN 1 END) AS "1",
    COUNT(CASE WHEN hora = 2 THEN 1 END) AS "2",
    COUNT(CASE WHEN hora = 3 THEN 1 END) AS "3",
    COUNT(CASE WHEN hora = 4 THEN 1 END) AS "4",
    COUNT(CASE WHEN hora = 5 THEN 1 END) AS "5",
    COUNT(CASE WHEN hora = 6 THEN 1 END) AS "6",
    COUNT(CASE WHEN hora = 7 THEN 1 END) AS "7",
    COUNT(CASE WHEN hora = 8 THEN 1 END) AS "8",
    COUNT(CASE WHEN hora = 9 THEN 1 END) AS "9",
    COUNT(CASE WHEN hora = 10 THEN 1 END) AS "10",
    COUNT(CASE WHEN hora = 11 THEN 1 END) AS "11",
    COUNT(CASE WHEN hora = 12 THEN 1 END) AS "12",
    COUNT(CASE WHEN hora = 13 THEN 1 END) AS "13",
    COUNT(CASE WHEN hora = 14 THEN 1 END) AS "14",
    COUNT(CASE WHEN hora = 15 THEN 1 END) AS "15",
    COUNT(CASE WHEN hora = 16 THEN 1 END) AS "16",
    COUNT(CASE WHEN hora = 17 THEN 1 END) AS "17",
    COUNT(CASE WHEN hora = 18 THEN 1 END) AS "18",
    COUNT(CASE WHEN hora = 19 THEN 1 END) AS "19",
    COUNT(CASE WHEN hora = 20 THEN 1 END) AS "20",
    COUNT(CASE WHEN hora = 21 THEN 1 END) AS "21",
    COUNT(CASE WHEN hora = 22 THEN 1 END) AS "22",
    COUNT(CASE WHEN hora = 23 THEN 1 END) AS "23"
FROM (
	SELECT
	    CASE STRFTIME('%w', order_purchase_timestamp)
	        WHEN '1' THEN 'Seg'
	        WHEN '2' THEN 'Ter'
	        WHEN '3' THEN 'Qua'
	        WHEN '4' THEN 'Qui'
	        WHEN '5' THEN 'Sex'
	        WHEN '6' THEN 'Sab'
	        WHEN '0' THEN 'Dom'
	        END AS dia_da_semana,
	    CAST(STRFTIME('%w', order_purchase_timestamp) AS INTEGER) AS dia_da_semana_int,
	    CAST(STRFTIME("%H", order_purchase_timestamp) AS INTEGER) AS hora
	FROM orders)
GROUP BY dia_da_semana_int
ORDER BY dia_da_semana_int


-- contagem de pedidos por cidade
SELECT 
    customer_city AS cidade_cliente,
    UPPER(customer_city) AS cidade,
    COUNT(orders.order_id) AS contagem_pedidos_cidade
FROM 
    customers
    JOIN orders USING (customer_id)
GROUP BY customer_city
ORDER BY contagem_pedidos_cidade DESC
LIMIT 10


-- estatísticas de preços através de sub consulta
SELECT
    MIN(preco_pedido) AS preco_min_pedido,
    ROUND(AVG(preco_pedido), 2) AS preco_medio_pedido,
    MAX(preco_pedido) AS preco_max_pedido
FROM (
    SELECT
        orders.order_id,
        SUM(order_items.price + order_items.freight_value) AS preco_pedido
    FROM orders
        JOIN order_items USING (order_id)
    GROUP BY orders.order_id)


-- obter o preço do item e o valor do frete
SELECT
    orders.order_id,
    SUM(price) AS preco_produto,
    SUM(freight_value) AS custo_frete
FROM
    orders
    JOIN order_items USING (order_id)
WHERE order_status = 'delivered'
GROUP BY orders.order_id LIMIT 100;


-- obter o ranking das categorias (utilizar na a subconsulta abaixo)
SELECT
    product_category_name AS categoria,
    SUM(price) AS vendas,
    RANK() OVER (ORDER BY SUM(price) DESC) AS classificacao
FROM order_items
    JOIN orders USING (order_id)
    JOIN products USING (product_id)
WHERE order_status = 'delivered'
GROUP BY product_category_name


-- obter o resumo de vendas por categoria com agrupamento de categorias
SELECT
    categoria,
    vendas
FROM (
	SELECT
	    product_category_name AS categoria,
	    SUM(price) AS vendas,
	    RANK() OVER (ORDER BY SUM(price) DESC) AS classificacao
	FROM order_items
	    JOIN orders USING (order_id)
	    JOIN products USING (product_id)
	WHERE order_status = 'delivered'
	GROUP BY product_category_name)
WHERE classificacao <= 15
	UNION ALL
	SELECT
	    'Outras categorias' AS categoria,
	    SUM(vendas) AS vendas
	FROM (
		SELECT
		    product_category_name AS categoria,
		    SUM(price) AS vendas,
		    RANK() OVER (ORDER BY SUM(price) DESC) AS classificacao
		FROM order_items
		    JOIN orders USING (order_id)
		    JOIN products USING (product_id)
		WHERE order_status = 'delivered'
		GROUP BY product_category_name)
	WHERE classificacao > 15


-- seleciona somente as categorias do ranking 15
SELECT 
	categoria
	FROM(
		SELECT
		    categoria,
		    vendas
		FROM (
			SELECT
			    product_category_name AS categoria,
			    SUM(price) AS vendas,
			    RANK() OVER (ORDER BY SUM(price) DESC) AS classificacao
			FROM order_items
			    JOIN orders USING (order_id)
			    JOIN products USING (product_id)
			WHERE order_status = 'delivered'
			GROUP BY product_category_name)
		WHERE classificacao <= 15)


-- Calcula o número da linha de cada produto dentro da sua categoria (ordenado pelo peso)
SELECT
    product_weight_g AS peso,
    product_category_name AS categoria,
    ROW_NUMBER() OVER(PARTITION BY product_category_name ORDER BY product_weight_g) AS categoria_linha_n,
    COUNT(*) OVER(PARTITION BY product_category_name) AS contagem_categoria
FROM
    products
    JOIN order_items USING (product_id)
WHERE
    product_category_name IN (
    	SELECT 
			categoria
		FROM (
			SELECT
			    categoria,
			    vendas
			FROM (
				SELECT
				    product_category_name AS categoria,
				    SUM(price) AS vendas,
				    RANK() OVER (ORDER BY SUM(price) DESC) AS classificacao
				FROM order_items
				    JOIN orders USING (order_id)
				    JOIN products USING (product_id)
				WHERE order_status = 'delivered'
				GROUP BY product_category_name)
			WHERE classificacao <= 15)
		) 
LIMIT 100
		

-- obter a mediana do número de vendas por categoria e classifica as categorias
SELECT categoria
FROM (
	SELECT
	    product_weight_g AS peso,
	    product_category_name AS categoria,
	    ROW_NUMBER() OVER(PARTITION BY product_category_name ORDER BY product_weight_g) AS categoria_linha_n,
	    COUNT(*) OVER(PARTITION BY product_category_name) AS contagem_categoria
	FROM
	    products
	    JOIN order_items USING (product_id)
	WHERE
	    product_category_name IN (
	    	SELECT 
				categoria
			FROM (
				SELECT
				    categoria,
				    vendas
				FROM (
					SELECT
					    product_category_name AS categoria,
					    SUM(price) AS vendas,
					    RANK() OVER (ORDER BY SUM(price) DESC) AS classificacao
					FROM order_items
					    JOIN orders USING (order_id)
					    JOIN products USING (product_id)
					WHERE order_status = 'delivered'
					GROUP BY product_category_name)
				WHERE classificacao <= 15)))
WHERE
-- Selecione a linha do meio para calcular a mediana, quando o número de produtos for ímpar: 
(contagem_categoria % 2 = 1 AND categoria_linha_n = (contagem_categoria + 1) / 2) OR
-- Selecione as duas linha do meio para calcular a mediana, quando o número de produtos for par: 
(contagem_categoria % 2 = 0 AND categoria_linha_n IN ((contagem_categoria / 2), (contagem_categoria / 2 + 1)))
GROUP BY categoria
ORDER BY AVG(peso)


-- geolocalização das localidades dos pedidos
SELECT
    geolocation_lat,
    geolocation_lng,
    geolocation_city,
    geolocation_state
FROM geolocation 
LIMIT 100


-- contagem de pedidos por cidade 2
SELECT 
    customer_city AS cidade_cliente,
    COUNT(orders.order_id) AS contagem_pedidos_cidade
FROM 
    customers
    JOIN orders USING (customer_id)
GROUP BY customer_city
ORDER BY contagem_pedidos_cidade DESC
LIMIT 100

```

Observações:

Para mais informações consulte o arquivo [LEIA-ME](https://github.com/thaleswillreis/Analise-Dados-E-commerce/blob/main/LEIAME.md) do projeto ou acesse o repositório do projeto: [https://github.com/thaleswillreis/Analise-Dados-E-commerce.git](https://github.com/thaleswillreis/Analise-Dados-E-commerce.git)
