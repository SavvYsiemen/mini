-- ## Small Project: Vehicle Information, Spares, and Accessories Data
-- This SQL project involves creating a database schema and populating it with sample data to manage vehicle information, spares, and accessories.

-- ### Step 1: Create Tables

-- Table 1: Vehicles
CREATE TABLE Vehicles (
    VehicleID INT PRIMARY KEY AUTO_INCREMENT,
    Make VARCHAR(50),
    Model VARCHAR(50),
    Year INT,
    VIN VARCHAR(17) UNIQUE,
    EngineType VARCHAR(50)
);

-- Table 2: Spares
CREATE TABLE Spares (
    SpareID INT PRIMARY KEY AUTO_INCREMENT,
    SpareName VARCHAR(100),
    VehicleID INT,
    Price DECIMAL(10, 2),
    Stock INT,
    FOREIGN KEY (VehicleID) REFERENCES Vehicles(VehicleID)
);

-- Table 3: Accessories
CREATE TABLE Accessories (
    AccessoryID INT PRIMARY KEY AUTO_INCREMENT,
    AccessoryName VARCHAR(100),
    VehicleID INT,
    Price DECIMAL(10, 2),
    Stock INT,
    FOREIGN KEY (VehicleID) REFERENCES Vehicles(VehicleID)
);

-- Table 4: Orders
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY AUTO_INCREMENT,
    CustomerName VARCHAR(100),
    OrderDate DATE,
    TotalAmount DECIMAL(10, 2)
);

-- Table 5: OrderDetails
CREATE TABLE OrderDetails (
    OrderDetailID INT PRIMARY KEY AUTO_INCREMENT,
    OrderID INT,
    ProductType ENUM('Spare', 'Accessory'),
    ProductID INT,
    Quantity INT,
    Price DECIMAL(10, 2),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
);

-- ### Step 2: Insert Sample Data

-- Insert into Vehicles
INSERT INTO Vehicles (Make, Model, Year, VIN, EngineType) VALUES
('Toyota', 'Camry', 2020, '1HGBH41JXMN109186', 'Hybrid'),
('Honda', 'Civic', 2018, '2HGEJ6671YH123456', 'Petrol'),
('Ford', 'Focus', 2022, '3FADP4EJ9KM123789', 'Diesel');

-- Insert into Spares
INSERT INTO Spares (SpareName, VehicleID, Price, Stock) VALUES
('Brake Pads', 1, 50.00, 100),
('Oil Filter', 2, 15.00, 200),
('Air Filter', 3, 20.00, 150);

-- Insert into Accessories
INSERT INTO Accessories (AccessoryName, VehicleID, Price, Stock) VALUES
('Floor Mats', 1, 30.00, 50),
('Phone Holder', 2, 10.00, 75),
('Roof Rack', 3, 150.00, 20);

-- Insert into Orders
INSERT INTO Orders (CustomerName, OrderDate, TotalAmount) VALUES
('John Doe', '2025-01-15', 80.00),
('Jane Smith', '2025-01-16', 150.00);

-- Insert into OrderDetails
INSERT INTO OrderDetails (OrderID, ProductType, ProductID, Quantity, Price) VALUES
(1, 'Spare', 1, 1, 50.00),
(1, 'Accessory', 1, 1, 30.00),
(2, 'Accessory', 3, 1, 150.00);

-- ### Step 3: Queries

-- Query 1: Fetch all vehicles with their spares and accessories
SELECT 
    v.VehicleID, v.Make, v.Model, v.Year, s.SpareName, a.AccessoryName
FROM 
    Vehicles v
LEFT JOIN 
    Spares s ON v.VehicleID = s.VehicleID
LEFT JOIN 
    Accessories a ON v.VehicleID = a.VehicleID;

-- Query 2: Total sales by product type
SELECT 
    ProductType, SUM(Quantity * Price) AS TotalSales
FROM 
    OrderDetails
GROUP BY 
    ProductType;

-- Query 3: Check stock for a specific vehicle
SELECT 
    v.Make, v.Model, s.SpareName, s.Stock AS SpareStock, a.AccessoryName, a.Stock AS AccessoryStock
FROM 
    Vehicles v
LEFT JOIN 
    Spares s ON v.VehicleID = s.VehicleID
LEFT JOIN 
    Accessories a ON v.VehicleID = a.VehicleID
WHERE 
    v.VehicleID = 1;
