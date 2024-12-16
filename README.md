# Swiggy Real Dataset Analysis project

![Project Banner Placeholder](https://github.com/sureka2003/Swiggy/blob/main/Swiggy.png.jpg)

Welcome to my **Swiggy Real Dataset Analysis project!** 🚀
This repository showcases my work on normalizing a denormalized Swiggy dataset and solving key business problems using structured tables such as **Orders, Customers, Deliveries, Riders, and Restaurants.** By applying data normalization techniques, I optimized data structure and carried out insightful analyses to address real-world challenges faced by food delivery platforms.
Dive into the project to explore how data engineering and analysis come together to drive impactful business solutions!


## Table of Contents
- [Introduction](#introduction)
- [Project Structure](#project-structure)
- [Database Schema](#database-schema)
- [Business Problems](#business-problems)
- [SQL Queries & Analysis](#sql-queries--analysis)
- [Getting Started](#getting-started)
- [Questions & Feedback](#questions--feedback)
- [Contact Me](#contact-me)

---

## Introduction

This project highlights advanced SQL proficiency through the analysis of a Swiggy dataset, encompassing key business domains like Orders, Customers, Deliveries, Riders, and Restaurants. By transforming a denormalized dataset into a structured, normalized format, the project enables comprehensive analysis to address critical business challenges. Leveraging foundational SQL techniques such as **Aggregations and Date Functions,** alongside advanced concepts like **Joins, Subqueries, and Window Functions.**The project delivers actionable insights into customer trends, operational efficiency, and performance metrics. This work demonstrates the ability to apply SQL to real-world datasets for strategic decision-making.

## Project Structure

1. **SQL Scripts**: Contains code to create the database schema and write queries for analysis.
2. **Dataset**: Real-time data representing swiggy food orders, customer details, deliveries, riders, and restaurant information.
3. **Analysis**: SQL queries crafted to solve business problems, each focusing on understanding food delivery platform sales and performance.

---

## Database Schema

Here’s an overview of the database structure:

### 1. **Customers Table**
- **customer_id**: Unique identifier for each customer
- **customer_name**: Name of the customer
- **reg_date**: Registration date of the customer

### 2. **Restaurants Table**
- **restaurant_id**: Unique identifier for each restaurant
- **restaurant_name**:Name of the restaurant
- **city**: Location of the restaurant
- **opening_hours**:Morning opening time
- **closing_hours**:Morning closing time
- **evening_opening_hours**:Evening opening time
- **evening_closing_hours**: Evening closing time


### 3. **Orders Table**
- **order_id**: Unique order identifier
- **order_item**: Description of the ordered item(s)
- **order_date**:  Date the order was placed
- **order_time**:  Time the order was placed
- **order_status**: Status of the order (e.g., Delivered, Cancelled)
- **total_amount**: Total amount for the order
- **customer_id**: Linked to `customers` table
- **restaurant_id**: Linked to `restaurants` table

### 4. **Riders Table**
- **rider_id**: Unique identifier for each rider
- **rider_name**:  Name of the rider
- **sign_up**: Date the rider signed up

### 5. **Deliveries Table**
- **delivery_id**: Unique identifier for each delivery
- **order_id**: Linked to the `orders` table
- **rider_id**: Linked to the `riders` table
- **delivery_status**: Status of the delivery 
- **delivery_time**: Time the order was delivered

## ERD (Entity-Relationship Diagram)

![ERD Placeholder](https://github.com/sureka2003/Swiggy/blob/main/Swiggy%20ERD.png)


## Business Problems

The following queries were created to solve specific business questions. Each query is designed to provide insights based on orders, customers, riders, deliveries and restaurant data.

### Easy 
1. ` Write a query to find the top 5 most frequently ordered dishes by the customer "Arjun Mehta" in the last 1 year.`
2. `Find the average order value (AOV) per customer who has placed more than 750 orders. Return: customer_name, AOV(average order value).`
3. `Determine each rider's average delivery time.`
4. `Identify the time slots during which the most orders are placed, based on 2-hour intervals`
5. `Rank restaurants by their total revenue from the last year.`
   
### Medium to Hard
1. ` Identify the most popular dish in each city based on the number of orders.`
2. `Calculate each restaurant's growth ratio based on the total number of delivered orders since its joining.`
3. `Calculate each rider's total monthly earnings,assuming they earn 8% of the order amount.`
4. `Find the number of 5-star, 4-star, and 3-star ratings each rider has.Riders receive ratings based on delivery time: ● 5-star: Delivered in less than 15 minutes ● 4-star: Delivered between 15 and 20 minutes ● 3-star: Delivered after 20 minutes`
5. `Identify sales trends by comparing each month's total sales to the previous month.`
   
---

## SQL Queries & Analysis

The `analysis.sql` file contains all SQL queries developed for this project. Each query corresponds to a business problem and demonstrates skills in SQL syntax, data filtering, aggregation, grouping, and ordering, as well as more intermediate topics such as Joins, Subqueries, Window Functions.

---

## Getting Started

### Prerequisites
- PostgreSQL (or any SQL-compatible database)
- Basic understanding of SQL

### Steps
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/flipkart-sql-project.git
   ```
2. **Set Up the Database**:
   - Run the `schema.sql` script to set up tables and insert sample data.

3. **Run Queries**:
   - Execute each query in `queries.sql` to explore and analyze the data.

---

## Questions & Feedback

Feel free to add your questions and code snippets below and submit them as issues for further feedback!

---

## Contact Me

📄 **[Resume](https://drive.google.com/file/d/1MprFBFLA7zugNGkSlYkCJwafZDyyVYur/view?usp=sharing)**  
📧 **[Email](mailto:surekafathimsf2003@gmail.com)**  
📞 **Phone**: +91 82481 25454

---


## Notice:
All customer names and data used in this project are computer-generated using AI and random
functions. They do not represent real data associated with swiggy or any other entity. This
project is solely for learning and educational purposes, and any resemblance to actual persons,
businesses, or events are purely coincidental.


