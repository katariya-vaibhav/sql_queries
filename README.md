# sql_queries


-- Create a database
CREATE DATABASE EventManagement;

-- Use the database
USE EventManagement;

-- Create LocationAdmins table
CREATE TABLE LocationAdmins (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL
);

-- Create Hosts table
CREATE TABLE Hosts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL
);

-- Create Events table
CREATE TABLE Events (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    location_admin_id INT NOT NULL,
    date DATE NOT NULL,
    FOREIGN KEY (location_admin_id) REFERENCES LocationAdmins(id) ON DELETE CASCADE
);

-- Create Bookings table
CREATE TABLE Bookings (
    id INT PRIMARY KEY AUTO_INCREMENT,
    event_id INT NOT NULL,
    host_id INT NOT NULL,
    booking_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (event_id) REFERENCES Events(id) ON DELETE CASCADE,
    FOREIGN KEY (host_id) REFERENCES Hosts(id) ON DELETE CASCADE
);

-- Insert sample data
INSERT INTO LocationAdmins (name, email) VALUES ('Alice', 'alice@example.com');
INSERT INTO Hosts (name, email) VALUES ('Bob', 'bob@example.com');
INSERT INTO Events (name, location_admin_id, date) VALUES ('Tech Conference', 1, '2025-04-15');
INSERT INTO Bookings (event_id, host_id) VALUES (1, 1);

-- Retrieve all events with their location admins
SELECT e.id, e.name AS event_name, e.date, la.name AS location_admin_name 
FROM Events e 
JOIN LocationAdmins la ON e.location_admin_id = la.id;

-- Retrieve all bookings with event and host details
SELECT b.id, e.name AS event_name, h.name AS host_name, b.booking_date 
FROM Bookings b
JOIN Events e ON b.event_id = e.id
JOIN Hosts h ON b.host_id = h.id;

-- Count the number of bookings for each event
SELECT e.name AS event_name, COUNT(b.id) AS total_bookings 
FROM Events e
LEFT JOIN Bookings b ON e.id = b.event_id
GROUP BY e.id, e.name;

-- Delete a specific booking
DELETE FROM Bookings WHERE id = 1;
