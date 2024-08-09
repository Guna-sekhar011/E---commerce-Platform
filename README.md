# E-commerce-Platform

Database Schema Design
Tables
Users

user_id (Primary Key)
username
password_hash
email
full_name
address
phone
created_at
updated_at
Products

product_id (Primary Key)
name
description
price
stock_quantity
category_id (Foreign Key to Categories table)
created_at
updated_at
Orders

order_id (Primary Key)
user_id (Foreign Key to Users table)
order_date
total_amount
status (e.g., pending, confirmed, shipped)
created_at
updated_at
Categories

category_id (Primary Key)
name
description
created_at
updated_at
Relationships
Users -> Orders: One-to-Many relationship (One user can have many orders).

Users.user_id -> Orders.user_id
Orders -> Products: Many-to-Many relationship (An order can contain multiple products and a product can be in multiple orders, but typically modeled with an intermediate table in relational databases).

Orders and Products tables will likely need an intermediate table like Order_Products to manage the many-to-many relationship, including additional attributes such as quantity per product in an order.
Additional Considerations
Indexes: Consider indexing user_id and order_id for efficient querying.
Data Integrity: Use foreign key constraints (FOREIGN KEY) to maintain referential integrity between tables.
Normalization: Ensure the database schema is normalized to reduce redundancy and improve data integrity.



Explanation:
Users Table: Stores information about registered users.
Categories Table: Holds product categories for organizing products.
Products Table: Contains details of products including name, price, and stock.
Orders Table: Tracks orders placed by users with status and total amount.
Order_Products Table: Intermediate table to manage the many-to-many relationship between orders and products, including the quantity of each product in an order.
