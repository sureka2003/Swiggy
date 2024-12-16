  
**Write a query to find the top 5 most frequently ordered dishes by the customer "Arjun Mehta" in the last 1 year.**

WITH t1 AS  
(select o.customer\_id,c.customer\_name,o.order\_item,  
count(o.order\_item) as order\_count  
from orders as o   
join customers as c on c.customer\_id \= o.customer\_id  
where customer\_name \= 'Arjun Mehta'  
AND o.order\_date \>= (CURRENT\_DATE \- INTERVAL '1 year')   
group by o.customer\_id,c.customer\_name,o.order\_item)

**Find the average order value (AOV) per customer who has placed more than 750 orders.**

select o.customer\_id,c.customer\_name,  
avg(o.total\_amount) as AOV,  
count (order\_id) as total\_orders  
from orders as o   
join customers as c on c.customer\_id \= o.customer\_id  
group by o.customer\_id,c.customer\_name  
HAVING count (order\_id) \> 750

**Determine each rider's average delivery time.**

select r.rider\_id,r.rider\_name,  
AVG(ABS(Extract (EPOCH from (delivery\_time-order\_time)) )/60) as avg\_delivery\_time\_in\_minutes  
from orders as o  
join deliveries as d on d.order\_id \= o.order\_id  
join riders as r on r.rider\_id \= d.rider\_id  
where d.delivery\_status \= 'Delivered'  
group by r.rider\_id,r.rider\_name  
                                           
**Identify the time slots during which the most orders are placed,**  
**based on 2-hour intervals.** 

SELECT   
    FLOOR(EXTRACT(HOUR FROM order\_time) / 2\) \* 2 AS time\_slot\_start,-- see notion sql notes for explanation   
    COUNT(order\_id) AS total\_orders  
FROM orders  
GROUP BY time\_slot\_start  
ORDER BY total\_orders DESC;

**Rank restaurants by their total revenue from the last year.**

WITH t1 AS  
(select o.restaurant\_id,r.restaurant\_name,r.city,  
sum(total\_amount) as total\_revenue  
from orders as o  
join restaurants as r on r.restaurant\_id \= o.restaurant\_id  
WHERE o.order\_date \>= (CURRENT\_DATE \- INTERVAL '1 year')  
group by o.restaurant\_id,r.restaurant\_name,r.city  
order by 4 DESC)

select \*,  
   DENSE\_RANK () OVER (PARTITION BY t1.city ORDER BY t1.total\_revenue DESC) as ranks  
from t1

**MEDIUM TO ADVANCED QUERIES:**  

**Identify the most popular dish in each city based on the number of orders.** 

WITH t1 AS  
(select o.order\_item,r.city,  
count(o.order\_id) as total\_orders  
from orders as o  
join restaurants as r on r.restaurant\_id \= o.restaurant\_id  
group by o.order\_item,r.city  
order by 3 DESC),  
t2 AS  
(select \*,  
    DENSE\_RANK () OVER (PARTITION BY city ORDER BY total\_orders DESC) as ranks  
from t1)  
select \* from t2  
where ranks \= 1

**Calculate each restaurant's growth ratio based on the total number of delivered orders since its joining.**

With orders\_delivered as   
       (select o.restaurant\_id,r.restaurant\_name,  
       count(d.order\_id) as delivered\_orders  
        from orders as o   
        join deliveries as d on d.order\_id \= o.order\_id  
        join restaurants as r on r.restaurant\_id \= o.restaurant\_id  
       where d.delivery\_status \= 'Delivered'  
        group by o.restaurant\_id,r.restaurant\_name),  
orders\_not\_delivered AS  
        (select o.restaurant\_id,r.restaurant\_name,  
         count(d.order\_id) as not\_delivered\_orders  
         from orders as o   
        join deliveries as d on d.order\_id \= o.order\_id  
        join restaurants as r on r.restaurant\_id \= o.restaurant\_id  
          where d.delivery\_status \= 'Not Delivered'   
        group by o.restaurant\_id,r.restaurant\_name),  
total\_orders as   
         (select restaurant\_id,  
         count(order\_id) as total\_orders  
        from orders  
         group by restaurant\_id)  
select od.restaurant\_id,od.restaurant\_name,  
ROUND((od.delivered\_orders::numeric/td.total\_orders::numeric)\*100,2) as growth\_ratio  
from orders\_delivered as od  
left join orders\_not\_delivered as ond  on od.restaurant\_id= ond.restaurant\_id  
right join total\_orders as td on td.restaurant\_id \= od.restaurant\_id

**Calculate each rider's total monthly earnings, assuming they earn 8% of the order amount.**   

WITH t1 AS  
(select d.rider\_id,r.rider\_name,  
DATE\_TRUNC('month', o.order\_date) AS month, \-- It truncates the order\_date to first day of the month  
sum(o.total\_amount) as order\_amount  
from orders as o  
join deliveries as d on d.order\_id \= o.order\_id  
join riders as r on r.rider\_id \= d.rider\_id  
group by d.rider\_id,r.rider\_name,DATE\_TRUNC('month', o.order\_date))

select \*,  
   ROUND(( 8 \* order\_amount ::numeric / 100),2) as total\_earnings  
from t1

**Find the number of 5-star, 4-star, and 3-star ratings each rider has.Riders receive ratings based on delivery time:● 5-star: Delivered in less than 15 minutes ● 4-star: Delivered between 15 and 20 minutes ● 3-star: Delivered after 20 minutes**

With t1 AS  
(select r.rider\_id,r.rider\_name,o.order\_time,d.delivery\_time,  
ROUND(AVG(ABS(Extract (Epoch from (delivery\_time \- order\_time)))/60),2) as timetaken\_for\_delivery  
from orders as o  
join deliveries as d on d.order\_id \= o.order\_id  
join riders as r on r.rider\_id \= d.rider\_id  
where d.delivery\_status \= 'Delivered'  
group by  r.rider\_id,r.rider\_name,o.order\_time,d.delivery\_time)

select rider\_id,  
    COUNT(CASE WHEN timetaken\_for\_delivery \< 15 THEN '5-star' END) AS five\_star\_ratings,  
    COUNT(CASE WHEN timetaken\_for\_delivery BETWEEN 15 AND 20 THEN '4-star' END) AS four\_star\_ratings,  
    COUNT(CASE WHEN timetaken\_for\_delivery \> 20 THEN '3-star' END) AS three\_star\_ratings  
from t1  
group by rider\_id

**Identify sales trends by comparing each month's total sales to the previous month.**  
    
WITH t1 AS  
(select DATE\_TRUNC('Month' , order\_date) as month,  
sum(total\_amount) as current\_month\_sales  
from orders   
group by DATE\_TRUNC('Month' , order\_date)),  
t2 AS  
(select\*,  
LAG (current\_month\_sales) over (order by month ASC ) as previous\_month\_sales  
from t1)

select \*,  
   (current\_month\_sales \- previous\_month\_sales) as sales\_trends  
from t2

