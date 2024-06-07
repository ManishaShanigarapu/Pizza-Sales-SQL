-- Database creation:Pizza Sales
create database pizzahut;
use pizzahut;

-- imported tables through csv:file
-- created table-for orders based on datatype:
create table orders
(
order_id int not null,
order_date date not null,
order_time time not null,
primary key(order_id)
);

show tables;  

-- seletion of Tables:
select * from order_details;
select * from orders;
select * from pizzas;
select * from pizzatype;

-- queries:
-- Retrieve the total number of orders placed.
SELECT 
    COUNT(*) AS number_of_orders_placed
FROM
    orders;

-- Calculate the total revenue generated from pizza sales.
SELECT 
    ROUND(SUM(order_details.quantity * pizzas.price),
            2) AS total_revenue
FROM
    pizzas
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id;
    
-- Identify the highest-priced pizza.
SELECT 
    pizzatype.name, pizzas.price
FROM
    pizzas
        JOIN
    pizzatype ON pizzatype.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC
LIMIT 1;

-- Identify the most common pizza size ordered.
SELECT 
    pizzas.size,
    COUNT(order_details.order_details_id) AS most_ordered_pizza
FROM
    pizzas
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizzas.size
ORDER BY most_ordered_pizza DESC
LIMIT 1;

-- List the top 5 most ordered pizza types along with their quantities.
SELECT 
    pizzatype.name, SUM(order_details.quantity) AS most_ordered
FROM
    pizzas
        JOIN
    pizzatype ON pizzatype.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzatype.name
ORDER BY most_ordered DESC
LIMIT 5;

-- Join the necessary tables to find the total quantity of each pizza category ordered.
SELECT 
    pizzatype.category,
    SUM(order_details.quantity) AS total_quantity
FROM
    pizzas
        JOIN
    Pizzatype ON pizzas.pizza_type_id = pizzatype.pizza_type_id
        JOIN
    order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizzatype.category
order by total_quantity desc
;

-- Determine the distribution of orders by hour of the day.
SELECT 
    HOUR(order_time) AS hour, COUNT(order_id) AS order_count
FROM
    orders
GROUP BY hour;

-- Join relevant tables to find the category-wise distribution of pizzas.
SELECT 
    category, COUNT(name) AS number_of_distribution
FROM
    pizzatype
GROUP BY pizzatype.category
;
-- Group the orders by date and calculate the average number of pizzas ordered per day.
SELECT 
    ROUND(AVG(quantity), 0)as avg_pizza_perday
FROM
    (SELECT 
        orders.order_date, SUM(order_details.quantity) AS quantity
    FROM
        orders
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY orders.order_date) AS order_quantity;
    

-- Determine the top 3 most ordered pizza types based on revenue.
select pizzatype.name,SUM(order_details.quantity * pizzas.price)AS total_revenue
FROM
    pizzas
    JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
    join pizzatype on  pizzatype.pizza_type_id=pizzas.pizza_type_id
    group by pizzatype.name
    order by total_revenue desc
    limit 3;


-- Calculate the percentage contribution of each pizza type to total revenue.
select pizzatype.category,round(sum(order_details.quantity*pizzas.price)/( SELECT 
    ROUND(SUM(order_details.quantity * pizzas.price),
            2) AS total_sales
FROM
    order_details
        JOIN
    pizzas ON pizzas.pizza_id=order_details.pizza_id )*100,2) as revenue from order_details
join pizzas on pizzas.pizza_id=order_details.pizza_id
join pizzatype on pizzatype.pizza_type_id= pizzas.pizza_type_id
group by pizzatype.category
order by revenue desc;


-- Analyze the cumulative revenue generated over time.
select order_date,sum(total_revenue) over(order by order_date)as cum_revenue
from
(select orders.order_date,
round(sum(order_details.quantity* pizzas.price),0)as total_revenue
from order_details 
join pizzas on order_details.pizza_id=pizzas.pizza_id
join orders on order_details.order_id=orders.order_id
group by orders.order_date) as sales;


-- Determine the top 3 most ordered pizza types based on revenue for each pizza category.

select name,revenue from
(select category,name,revenue,
rank () over(partition by category order by revenue desc)
as rn from 
(select pizzatype.category,pizzatype.name,sum(order_details.quantity*pizzas.price)as revenue
from order_details 
join pizzas on pizzas.pizza_id=order_details.pizza_id
join pizzatype on pizzatype.pizza_type_id=pizzas.pizza_type_id
group by pizzatype.category ,pizzatype.name)
as a)as b where rn <=3;
















    
    

