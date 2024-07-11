# E---commerce-Platform

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



create the SQL script to define the tables based on the schema we discussed:

-- Create Users table
CREATE TABLE Users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    full_name VARCHAR(100),
    address VARCHAR(255),
    phone VARCHAR(20),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Create Categories table
CREATE TABLE Categories (
    category_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Create Products table
CREATE TABLE Products (
    product_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL,
    stock_quantity INT DEFAULT 0,
    category_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES Categories(category_id)
);

-- Create Orders table
CREATE TABLE Orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10, 2) NOT NULL,
    status ENUM('pending', 'confirmed', 'shipped') DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

-- Create Order_Products table for Many-to-Many relationship between Orders and Products
CREATE TABLE Order_Products (
    order_id INT,
    product_id INT,
    quantity INT NOT NULL,
    PRIMARY KEY (order_id, product_id),
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);


Explanation:
Users Table: Stores information about registered users.
Categories Table: Holds product categories for organizing products.
Products Table: Contains details of products including name, price, and stock.
Orders Table: Tracks orders placed by users with status and total amount.
Order_Products Table: Intermediate table to manage the many-to-many relationship between orders and products, including the quantity of each product in an order.
