# Supermart-Database
Welcome to the Supermart Database project! This repository is a comprehensive showcase of SQL commands and database management techniques applied to a fictional supermarket database.

select * from customer;
select * from product;
select * from sales;

Select * from customer where city IN ('Philadelphia','Seattle');

select * from customer where city = 'Philadelphia' or City= 'Seattle';

/* BETWEEEN */

Select * from customer where age between 20 and 30;

Select * from customer where age Not between 20 and 30;

select * from sales where ship_date between '2015-04-01' and '2016-04-01';

/* LIKE */

Select * from customer where customer_name like 'J%';

select * from customer where customer_name like '%Nelson%';

Select * from customer where customer_name like '____ %';

Select distinct city from customer where city not like 'S%';

Select * from customer where customer_name like 'G\%';

Select Distinct City from customer where region IN ('North','East');

Select * from sales where Sales between 100 and 500;

Select * from customer where customer_name like '% ____';

/* ORDER BY */

Select * from customer where State ='California' order by Customer_name;

Select * from customer where State= 'California' Order by customer_name Desc;

Select * from customer order by city Asc, customer_name desc;

Select * from customer order by 2 Asc;

Select * from customer order by age Desc;

/* LIMIT */

Select * from customer where age>= 25 order by age Desc Limit 8;

Select * from customer where age >25 order by age Asc limit 10;

Select * from Sales where Discount>0 order by discount Desc;

Select * from Sales where discount >0 order by Discount Desc Limit 10;

/* ALIAS (AS)*/

select customer_id As "Serial Number",customer_name As Name, Age as customer_age from customer;

/* COUNT */

Select Count(*) from Sales;

Select count(order_line) as "Number of products ordered", count(Distinct order_id) as "number of orders" 
from sales where Customer_id ='CG-12520';

/* SUM */

Select Sum (Profit) as "Total Profit" from Sales;

Select sum (Quantity) As "Total Quantity" from sales where product_id = 'FUR-TA-10000577';

/* AVERAGE */

Select Avg(age) as "Average Coustomer Age" from customer;
Select Avg(Sales*0.10)As "Average Commision Value" from Sales;

/* MIN AND MAX */

Select Min(Sales) as "Minimum Sales Value JUNE 15" from Sales where order_date between '2015-06-01' and '2015-06-30';

Select Sales from sales where order_date between '2015-06-01' and '2015-06-30' order by Sales Asc;

Select Max(Sales) as "Maximum Sales Value JUNE 15" from Sales where order_date between '2015-06-01' and '2015-06-30';

Select Sales from sales where order_date between '2015-06-01' and '2015-06-30' order by Sales Desc;

Select Sum(Sales) from Sales;

Select Count(*) from Customer where Age between 20 and 30;

Select Avg(Age) from Customer where Region = 'East';

Select Min(age), Max(age) from customer where city= 'Philadelphia';

/* GROUP BY */

Select region, count(customer_id) as customer_count from Customer group by region; 

Select Product_id, count(Quantity) as quantity_sold from sales group by product_id order by quantity_Sold Desc;

select customer_id, Min(sales) as minimum_sales, Max(Sales) as Maximum_sales, avg(Sales) as Average_Sales, Sum(Sales)
As Total_Sales from Sales Group by customer_id order by Total_Sales Desc limit 5;

/* HAVING */

Select region, count(Customer_id) As Customer_count from Customer Group by Region having count(Customer_id)>200; 

Select region, count(customer_id) as customer_count from customer where customer_name like 'A%' group by region ;

Select region, count(Customer_id) as customer_count from customer where customer_name like 'A%' group by region 
having count(customer_id)> 15;

Select * from Sales;

Select Product_id, Sum(Sales) as Total_Sales, Sum(Quantity) As Total_Sales_Qunatity, count(Order_id) As Number_of_Orders, 
Max(Sales)As Max_Sales_Value, Min(Sales) as Min_Sales_value, Avg(Sales) as Average_Sales_Value from Sales group by Product_id 
Order by Total_Sales Desc;
Select Product_id, Sum(Quantity) As Total_Quantity from Sales Group by Product_id Having count(Quantity)>10;

/* CASE WHEN */

Select *, Case 
when Age<30 Then 'Young' 
when age>60 then 'Senior citizen'
Else 'Middle Aged'
End as Age_category
from Customer;

/* JOINS */

/* Creating sales table of year 2015 */
Create table sales_2015 as select * from sales where ship_date between '2015-01-01' and '2015-12-31';
select count(*) from sales_2015; 
select count(distinct customer_id) from sales_2015;

/* Customers with age between 20 and 60 */
create table customer_20_60 as select * from customer where age between 20 and 60;
select count (*) from customer_20_60;

/* INNER JOIN */

select 
a.Order_line,
a.Product_id,
a.CUstomer_id,
a.Sales,
b.customer_Name,
b.age
from sales_2015 as a
inner join customer_20_60 as b 
on a.customer_id = b.customer_id
order by customer_id;

/* LEFT JOIN */

Select 
a.Order_line,
a.product_id,
a.customer_id,
a.sales,
b.customer_name,
b.Age
from sales_2015 as a
Left join customer_20_60 as b
on a.customer_id = b.customer_id
order by customer_id;

/* RIGHT JOIN */

select a.Order_line,
a.product_id,
b.customer_id,
a.Sales,
b.customer_name,
b.age
from Sales_2015 as a
right join customer_20_60 as b 
On a.customer_id = b.customer_id 
order by customer_id;

/* FULL JOIN */

select 
a.order_line,
a.product_id,
a.customer_id,
a.sales,
b.customer_name,
b.age,
b.customer_id
from sales_2015 as a 
Full join customer_20_60 as b 
on a.customer_id = b.customer_id 
order by a.customer_id, b.customer_id;

/* INTERSECT */

Select customer_id from Sales_2015
INTERSECT
select customer_id from customer_20_60;

/* EXCEPT */

Select customer_id from sales_2015
Except 
Select Customer_id from Customer_20_60
order by customer_id;

/* UNION */

Select customer_id from sales_2015
UNION 
Select customer_id from customer_20_60
ORDER BY customer_id;

select *  from customer_20_60;
select * from sales_2015;

Select b.state, sum(sales) as total_sales
from sales_2015 as a left join customer_20_60 as b
On a.customer_id = b.customer_id
group by b.state;

select * from product;
select * from sales;

select a.*,sum(b.sales)as total_sales, sum(b.quantity)as toal_quantity
from product as a left join sales as b 
on a.product_id = b.product_id
group by a.product_id;

/* SUBQUERY */

select * from sales where customer_id in (
select distinct(customer_id) from customer where age>60);

select a.product_id,
a.product_name,
a.category,
b.quantity
from product as a
left join(select product_id, Sum(quantity) as quantity from sales group by product_id ) as b 
on a.product_id = b.product_id
order by b.quantity desc;

Select Customer_id, order_line, (Select customer_name from customer where sales.customer_id = customer.customer_id)
from sales order by customer_id;

Select c.customer_name, c.age,sp.* from customer as c 
right join (select s.*,p.product_name,p.category
		   from sales as s
		   left join product as p
		   on s.product_id = p.product_id) as sp
		   on c.customer_id = sp.customer_id;
		   
/* VIEW */

Create view logistics as
select a.order_line,
a.order_id,
b.customer_name,
b.city,
b.state,
b.country
from sales as a left join customer as b 
on a.customer_id = b.customer_id
order by a.order_line;

select * from logistics;

/* DROP OR UPDATE VIEW */

Drop view logistics;

Create view Daily_Billing as
select order_line, product_id, sales, discount from sales where order_date IN(select max(order_date) from sales);

select * from daily_billing;

Drop view Daily_Billing;

/* SUBSTRING */

select customer_id, customer_name,
Substring(customer_id for 2) as cust_group
from customer
where substring(customer_id for 2)='AB';

/* ROUND */

Select order_line, Sales, Round(sales) from sales;


/* WINDOWS FUNCTIONS */
 /* ROW_NUMBER*/
 
Select a.*, b.Order_num, b.Sales_total, b.Quantity_total, b.Profit_total
from customer as a
left join (Select customer_id, count(distinct Order_Id) as Order_num, Sum(sales) As Sales_total, 
		   Sum(Quantity) as Quantity_total, Sum(Profit) as Profit_Total from sales group by Customer_id) as b
ON a.customer_id = b.customer_id;		   
		   
Create Table Customer_order as(Select a.*, b.Order_num, b.Sales_total, b.Quantity_total, b.Profit_total
from customer as a
left join (Select customer_id, count(distinct Order_Id) as Order_num, Sum(sales) As Sales_total, 
		   Sum(Quantity) as Quantity_total, Sum(Profit) as Profit_Total from sales group by Customer_id) as b
ON a.customer_id = b.customer_id);

Select * from customer_order;

Select customer_id, Customer_name, state, order_num, row_number() Over(Partition by state order by order_num Desc) as row_n
from customer_order;

Select * from (Select customer_id, Customer_name, state, order_num, row_number() Over(Partition by state order by order_num Desc) as row_n
from customer_order) as a where a.row_n<=3;

/* RANK AND DENSE RANK */

Select customer_id, Customer_name, State, Order_num, 
Row_number() Over(Partition by state Order by order_num desc) as row_n,
Rank() Over(Partition by State Order by Order_num desc) as rank_n,
Dense_rank() Over(Partition by State Order by Order_num desc) as Dense_rank_n
from customer_order;

/* NTILE*/

Select Customer_id, customer_name, State,order_num,
Ntile(5) over(Partition by state order by order_num desc) as ntile_n
from customer_order;

/* AVERAGE */

Select customer_id, customer_name, State, Sales_total as revenue,
Avg(Sales_total) over (Partition by state)as avg_revenue 
from customer_order;

--Customer with less than avg revenue

Select * from (Select customer_id, customer_name, State, Sales_total as revenue,
Avg(Sales_total) over (Partition by state)as avg_revenue 
from customer_order) as a where a.Revenue<a.avg_revenue;

/* COUNT */

select customer_id, customer_name, state,
Count(customer_id) over(Partition by state) as count_cust
from customer_order;
