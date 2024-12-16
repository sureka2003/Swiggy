SETUP SWIGGY DATABASE SCHEMA

–Customers table  
```sql
create table customers  
      ( customer\_id	 INT primary key,  
	   customer\_name VARCHAR (20)	,  
	    reg\_date DATE   
      );
```
–Restaurants table  
```sql
create table restaurants   
       (restaurant\_id	INT primary key,  
	   restaurant\_name	VARCHAR (35),  
	   city	VARCHAR (25),  
	   opening\_hours TIME,	  
	   closing\_hours TIME,	  
	   evening\_opening\_hours TIME,  
	   evening\_closing\_hours TIME  
      );
```
–Orders table  
```sql
create table orders (  
       order\_id	INT primary key,  
	   order\_item VARCHAR (100),  
	   order\_date DATE,	  
	   order\_time TIME,  
	   order\_status	VARCHAR(25),  
	   total\_amount	INT,  
	   customer\_id	INT references customers(customer\_id),  
	   restaurant\_id INT references restaurants (restaurant\_id)

);
```
–Riders table  
```sql
CREATE TABLE riders (  
    rider\_id INT PRIMARY KEY,  
    rider\_name VARCHAR(45),  
    sign\_up DATE  
);
```
–Deliveries table  
```sql
CREATE TABLE deliveries (  
    order\_id INT REFERENCES orders(order\_id),  
    rider\_id INT REFERENCES riders(rider\_id),  
    delivery\_id INT PRIMARY KEY,  
    delivery\_status VARCHAR(50),  
    delivery\_time TIME  
     
);  
```
