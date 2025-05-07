CREATE DATABASE autorush CHARACTER SET utf8mb4 COLLATE utf8mb4_hungarian_ci;
USE autorush;

-- Users tábla
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    phone VARCHAR(20),
    driver_license VARCHAR(50),
    role ENUM('user', 'admin') DEFAULT 'user',
    is_active BOOLEAN DEFAULT TRUE,
    login_attempts INT DEFAULT 0,
    last_attempt TIMESTAMP NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Brands tábla
CREATE TABLE brands (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    logo VARCHAR(255)
);

-- Cars tábla
CREATE TABLE cars (
    id INT AUTO_INCREMENT PRIMARY KEY,
    brand_id INT NOT NULL,
    model VARCHAR(50) NOT NULL,
    year INT NOT NULL,
    type ENUM('SUV', 'Sedan', 'Hatchback', 'Sports', 'Luxury') NOT NULL,
    seats INT NOT NULL,
    price_per_day DECIMAL(10,2) NOT NULL,
    image VARCHAR(255) NOT NULL,
    available BOOLEAN DEFAULT TRUE,
    description TEXT,
    FOREIGN KEY (brand_id) REFERENCES brands(id)
);

-- Bookings tábla
CREATE TABLE bookings (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    car_id INT NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    total_price DECIMAL(10,2) NOT NULL,
    status ENUM('pending', 'confirmed', 'cancelled', 'completed') DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (car_id) REFERENCES cars(id)
);

-- Access logs tábla
CREATE TABLE access_logs (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    action VARCHAR(50),
    ip_address VARCHAR(45),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Alap admin felhasználó
INSERT INTO users (name, email, password, role) 
VALUES (
    'Admin', 
    'admin@autorush.hu', 
    '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi',  -- jelszó: "password"
    'admin'
);
