-- Database Setup
CREATE DATABASE ECommerceDB;
USE ECommerceDB;

-- Table Creation
CREATE TABLE Customer (
    CustomerID INT PRIMARY KEY AUTO_INCREMENT,
    First_Name VARCHAR(30) NOT NULL,
    Last_Name VARCHAR(30),
    Email VARCHAR(50) NOT NULL UNIQUE,
    Phone BIGINT(10) NOT NULL UNIQUE
);

CREATE TABLE Address (
    Address_ID INT PRIMARY KEY AUTO_INCREMENT,
    Customer_ID INT NOT NULL,
    PIN VARCHAR(6) NOT NULL,
    City VARCHAR(50) NOT NULL,
    Street VARCHAR(100),
    FOREIGN KEY (Customer_ID) REFERENCES Customer(CustomerID)
        ON DELETE CASCADE
);

CREATE TABLE Category (
    Category_ID INT PRIMARY KEY AUTO_INCREMENT,
    Category_Name VARCHAR(50)
);

CREATE TABLE Seller (
    Seller_ID INT PRIMARY KEY AUTO_INCREMENT,
    Seller_Name VARCHAR(50) NOT NULL,
    Phone VARCHAR(10) NOT NULL UNIQUE
);

CREATE TABLE Product (
    Product_ID INT PRIMARY KEY AUTO_INCREMENT,
    Product_Name VARCHAR(100) NOT NULL,
    Price DECIMAL(10,2) NOT NULL,
    Category_ID INT,
    Seller_ID INT NOT NULL,
    FOREIGN KEY (Category_ID) REFERENCES Category(Category_ID)
        ON DELETE SET NULL,
    FOREIGN KEY (Seller_ID) REFERENCES Seller(Seller_ID)
        ON DELETE CASCADE
);

CREATE TABLE Orders (
    Order_ID INT PRIMARY KEY AUTO_INCREMENT,
    Customer_ID INT NOT NULL,
    Address_ID INT,
    Order_Date DATE NOT NULL,
    Order_Status VARCHAR(30) NOT NULL,
    FOREIGN KEY (Customer_ID) REFERENCES Customer(CustomerID)
        ON DELETE CASCADE,
    FOREIGN KEY (Address_ID) REFERENCES Address(Address_ID)
        ON DELETE SET NULL
);

CREATE TABLE Order_Item (
    Order_ID INT,
    Product_ID INT,
    Quantity INT NOT NULL,
    Price DECIMAL(10,2) NOT NULL,
    PRIMARY KEY (Order_ID, Product_ID),
    FOREIGN KEY (Order_ID) REFERENCES Orders(Order_ID)
        ON DELETE CASCADE,
    FOREIGN KEY (Product_ID) REFERENCES Product(Product_ID)
        ON DELETE SET NULL
);

CREATE TABLE Payment (
    Payment_ID INT PRIMARY KEY AUTO_INCREMENT,
    Order_ID INT NOT NULL UNIQUE,
    Method VARCHAR(30) NOT NULL,
    FOREIGN KEY (Order_ID) REFERENCES Orders(Order_ID)
        ON DELETE CASCADE
);

CREATE TABLE Delivery (
    DELIVERY_ID INT PRIMARY KEY AUTO_INCREMENT,
    Order_ID INT NOT NULL UNIQUE,
    Tracking_No VARCHAR(50) NOT NULL UNIQUE,
    FOREIGN KEY (Order_ID) REFERENCES Orders(Order_ID)
        ON DELETE CASCADE
);

-- Sample Data Insertion
INSERT INTO Customer (First_Name, Last_Name, Email, Phone) VALUES
('Priya', 'Sharma', 'priya@gmail.com', '9876543210'),
('Rahul', 'Verma', 'rahul@gmail.com', '9876543211'),
('Ananya', 'Singh', 'ananya@gmail.com', '9876543212'),
('Arjun', 'Mehta', 'arjun@gmail.com', '9876543213'),
('Neha', 'Gupta', 'neha@gmail.com', '9876543214');

INSERT INTO Address (Customer_ID, PIN, City, Street) VALUES
(1, '11001', 'Delhi', 'MG Road'),
(2, '30201', 'Jaipur', 'MI Road'),
(3, '22601', 'Mumbai', 'Link Road'),
(4, '40001', 'Lucknow', 'Hazratganj'),
(5, '56001', 'Bangalore', 'Brigade Road');

INSERT INTO Category (Category_Name) VALUES
('Electronics'),
('Clothing'),
('Books'),
('Furniture'),
('Beauty');

INSERT INTO Seller (Seller_Name, Phone) VALUES
('Tech World', '9080706050'),
('Fashion Hub', '9181716151'),
('Book Store', '9282726252'),
('Home Decor', '9383736353'),
('Beauty Point', '9484746454');

INSERT INTO Product (Product_Name, Price, Category_ID, Seller_ID) VALUES
('Laptop', 55000.00, 1, 1),
('T-Shirt', 399.00, 2, 2),
('Python Book', 499.00, 2, 2),
('Study Table', 4500.00, 4, 4),
('Face Wash', 299.00, 5, 5);

INSERT INTO Orders (Customer_ID, Address_ID, Order_Date, Order_Status) VALUES
(1, 1, '2026-08-23', 'Confirmed'),
(2, 2, '2026-08-23', 'Shipped'),
(3, 3, '2026-08-21', 'Delivered'),
(4, 4, '2026-08-20', 'Processing'),
(5, 5, '2026-08-19', 'Cancelled');

INSERT INTO Order_Item (Order_ID, Product_ID, Quantity, Price) VALUES
(1, 1, 1, 55000.00),
(2, 2, 2, 399.00),
(3, 3, 3, 499.00),
(4, 4, 4, 4500.00),
(5, 5, 5, 299.00);

INSERT INTO Payment (Order_ID, Method) VALUES
(1, 'UPI'),
(2, 'Card'),
(3, 'Cash'),
(4, 'UPI'),
(5, 'Net Banking');

INSERT INTO Delivery (Order_ID, Tracking_No) VALUES
(1, 'TRK1001'),
(2, 'TRK1002'),
(3, 'TRK1003'),
(4, 'TRK1004'),
(5, 'TRK1005');

-- Referential Integrity Demonstrations

-- 1. Referential Integrity Violation (Fails because Customer_ID 10 does not exist)
INSERT INTO Orders (Customer_ID, Address_ID, Order_Date, Order_Status) 
VALUES (10, 10, '2026-08-13', 'Confirmed');

-- 2. ON DELETE SET NULL Demonstration
DELETE FROM Category WHERE Category_ID = 15;

-- 3. ON DELETE CASCADE Demonstration
DELETE FROM Customer WHERE CustomerID = 1;
