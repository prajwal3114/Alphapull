# 🗄️ Database Setup Guide

Complete guide to set up MySQL database for Alpha platform.

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Database Creation](#database-creation)
- [Schema Setup](#schema-setup)
- [Sample Data](#sample-data)
- [Database Management](#database-management)
- [Backup & Restore](#backup--restore)
- [Troubleshooting](#troubleshooting)

## 🎯 Prerequisites

- **MySQL** (v8.0 or higher)
- **MySQL Workbench** (recommended) or command-line access
- **Administrator/root access** to MySQL
- **Backend setup completed** (for connection testing)

### Check MySQL Installation

```bash
# Check MySQL version
mysql --version
# Should output: mysql Ver 8.0.x or higher

# Check if MySQL is running
# Windows
sc query MySQL

# Linux
sudo systemctl status mysql

# macOS
brew services list | grep mysql
```

## 📦 Installation

### Windows (XAMPP)

1. **Download XAMPP**
   - Visit [https://www.apachefriends.org/](https://www.apachefriends.org/)
   - Download XAMPP for Windows
   - Install with MySQL component

2. **Start MySQL**
   - Open XAMPP Control Panel
   - Click "Start" next to MySQL
   - Default credentials: root (no password)

### Windows (Standalone)

1. **Download MySQL Installer**
   - Visit [https://dev.mysql.com/downloads/installer/](https://dev.mysql.com/downloads/installer/)
   - Download MySQL Installer for Windows
   - Run installer and select "Server only" or "Full"

2. **Configure MySQL**
   - Set root password
   - Configure as Windows Service
   - Complete installation

### Linux (Ubuntu/Debian)

```bash
# Update package index
sudo apt update

# Install MySQL Server
sudo apt install mysql-server

# Secure installation
sudo mysql_secure_installation

# Start MySQL service
sudo systemctl start mysql
sudo systemctl enable mysql
```

### macOS

```bash
# Install using Homebrew
brew install mysql

# Start MySQL service
brew services start mysql

# Secure installation
mysql_secure_installation
```

## 🔧 Database Creation

### Step 1: Access MySQL

```bash
# Using root user
mysql -u root -p
# Enter your root password when prompted
```

### Step 2: Create Database

```sql
-- Create database
CREATE DATABASE alpha_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Verify database creation
SHOW DATABASES;

-- Use the database
USE alpha_db;
```

### Step 3: Create Database User (Recommended)

```sql
-- Create dedicated user
CREATE USER 'alpha_user'@'localhost' IDENTIFIED BY 'secure_password_here';

-- Grant privileges
GRANT ALL PRIVILEGES ON alpha_db.* TO 'alpha_user'@'localhost';

-- Apply changes
FLUSH PRIVILEGES;

-- Verify user creation
SELECT User, Host FROM mysql.user WHERE User = 'alpha_user';
```

## 📊 Schema Setup

### Complete Database Schema

```sql
-- ============================================
-- USERS TABLE
-- ============================================
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    role ENUM('owner', 'renter', 'admin') NOT NULL DEFAULT 'renter',
    full_name VARCHAR(255) NOT NULL,
    phone VARCHAR(20),
    profile_image VARCHAR(500),
    email_verified BOOLEAN DEFAULT FALSE,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    last_login TIMESTAMP NULL,
    INDEX idx_email (email),
    INDEX idx_role (role),
    INDEX idx_created_at (created_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- ============================================
-- SPACES TABLE
-- ============================================
CREATE TABLE spaces (
    id INT AUTO_INCREMENT PRIMARY KEY,
    owner_id INT NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    address TEXT NOT NULL,
    city VARCHAR(100) NOT NULL,
    state VARCHAR(100),
    zip_code VARCHAR(20),
    country VARCHAR(100) DEFAULT 'India',
    latitude DECIMAL(10, 8) NOT NULL,
    longitude DECIMAL(11, 8) NOT NULL,
    price_per_hour DECIMAL(10, 2) NOT NULL,
    space_type ENUM('parking', 'storage', 'warehouse', 'other') DEFAULT 'parking',
    capacity INT DEFAULT 1,
    available_capacity INT DEFAULT 1,
    dimensions VARCHAR(100),
    amenities JSON,
    images JSON,
    status ENUM('pending', 'approved', 'rejected', 'inactive') DEFAULT 'pending',
    rejection_reason TEXT,
    is_featured BOOLEAN DEFAULT FALSE,
    rating DECIMAL(3, 2) DEFAULT 0.00,
    total_bookings INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    approved_at TIMESTAMP NULL,
    FOREIGN KEY (owner_id) REFERENCES users(id) ON DELETE CASCADE,
    INDEX idx_owner (owner_id),
    INDEX idx_location (latitude, longitude),
    INDEX idx_status (status),
    INDEX idx_space_type (space_type),
    INDEX idx_price (price_per_hour),
    INDEX idx_rating (rating)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- ============================================
-- BOOKINGS TABLE
-- ============================================
CREATE TABLE bookings (
    id INT AUTO_INCREMENT PRIMARY KEY,
    space_id INT NOT NULL,
    renter_id INT NOT NULL,
    start_time DATETIME NOT NULL,
    end_time DATETIME NOT NULL,
    total_hours DECIMAL(10, 2) NOT NULL,
    price_per_hour DECIMAL(10, 2) NOT NULL,
    total_amount DECIMAL(10, 2) NOT NULL,
    payment_status ENUM('pending', 'completed', 'failed', 'refunded') DEFAULT 'pending',
    booking_status ENUM('pending', 'confirmed', 'active', 'completed', 'cancelled') DEFAULT 'pending',
    cancellation_reason TEXT,
    stripe_session_id VARCHAR(255),
    stripe_payment_intent VARCHAR(255),
    check_in_time DATETIME NULL,
    check_out_time DATETIME NULL,
    rating INT CHECK (rating >= 1 AND rating <= 5),
    review TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (space_id) REFERENCES spaces(id) ON DELETE CASCADE,
    FOREIGN KEY (renter_id) REFERENCES users(id) ON DELETE CASCADE,
    INDEX idx_space (space_id),
    INDEX idx_renter (renter_id),
    INDEX idx_dates (start_time, end_time),
    INDEX idx_payment_status (payment_status),
    INDEX idx_booking_status (booking_status),
    INDEX idx_created_at (created_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- ============================================
-- PAYMENTS TABLE
-- ============================================
CREATE TABLE payments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    booking_id INT NOT NULL,
    user_id INT NOT NULL,
    amount DECIMAL(10, 2) NOT NULL,
    currency VARCHAR(3) DEFAULT 'INR',
    payment_method ENUM('stripe', 'razorpay', 'cash') DEFAULT 'stripe',
    transaction_id VARCHAR(255) UNIQUE,
    stripe_session_id VARCHAR(255),
    stripe_payment_intent VARCHAR(255),
    status ENUM('pending', 'processing', 'completed', 'failed', 'refunded') DEFAULT 'pending',
    payment_date TIMESTAMP NULL,
    refund_amount DECIMAL(10, 2) DEFAULT 0.00,
    refund_date TIMESTAMP NULL,
    metadata JSON,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (booking_id) REFERENCES bookings(id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    INDEX idx_booking (booking_id),
    INDEX idx_user (user_id),
    INDEX idx_transaction (transaction_id),
    INDEX idx_status (status),
    INDEX idx_payment_date (payment_date)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- ============================================
-- AVAILABILITY TABLE
-- ============================================
CREATE TABLE availability (
    id INT AUTO_INCREMENT PRIMARY KEY,
    space_id INT NOT NULL,
    date DATE NOT NULL,
    start_time TIME NOT NULL,
    end_time TIME NOT NULL,
    is_available BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (space_id) REFERENCES spaces(id) ON DELETE CASCADE,
    UNIQUE KEY unique_space_time (space_id, date, start_time),
    INDEX idx_space_date (space_id, date)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- ============================================
-- REVIEWS TABLE
-- ============================================
CREATE TABLE reviews (
    id INT AUTO_INCREMENT PRIMARY KEY,
    booking_id INT NOT NULL,
    space_id INT NOT NULL,
    reviewer_id INT NOT NULL,
    rating INT NOT NULL CHECK (rating >= 1 AND rating <= 5),
    review TEXT,
    owner_response TEXT,
    is_verified BOOLEAN DEFAULT FALSE,
    helpful_count INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (booking_id) REFERENCES bookings(id) ON DELETE CASCADE,
    FOREIGN KEY (space_id) REFERENCES spaces(id) ON DELETE CASCADE,
    FOREIGN KEY (reviewer_id) REFERENCES users(id) ON DELETE CASCADE,
    UNIQUE KEY unique_booking_review (booking_id),
    INDEX idx_space (space_id),
    INDEX idx_reviewer (reviewer_id),
    INDEX idx_rating (rating)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- ============================================
-- NOTIFICATIONS TABLE
-- ============================================
CREATE TABLE notifications (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    title VARCHAR(255) NOT NULL,
    message TEXT NOT NULL,
    type ENUM('booking', 'payment', 'review', 'system', 'promotion') NOT NULL,
    reference_id INT,
    is_read BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    INDEX idx_user (user_id),
    INDEX idx_read (is_read),
    INDEX idx_created_at (created_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- ============================================
-- CHAT_MESSAGES TABLE (For chatbot)
-- ============================================
CREATE TABLE chat_messages (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    session_id VARCHAR(255) NOT NULL,
    message TEXT NOT NULL,
    response TEXT NOT NULL,
    intent VARCHAR(100),
    confidence DECIMAL(5, 4),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE SET NULL,
    INDEX idx_user (user_id),
    INDEX idx_session (session_id),
    INDEX idx_created_at (created_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- ============================================
-- AUDIT_LOG TABLE
-- ============================================
CREATE TABLE audit_log (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    action VARCHAR(100) NOT NULL,
    table_name VARCHAR(100),
    record_id INT,
    old_value JSON,
    new_value JSON,
    ip_address VARCHAR(45),
    user_agent TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE SET NULL,
    INDEX idx_user (user_id),
    INDEX idx_action (action),
    INDEX idx_table (table_name),
    INDEX idx_created_at (created_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

### Run Schema Setup

**Option 1: From MySQL Command Line**

```sql
-- Copy and paste the schema SQL above into MySQL prompt
-- Or source from file:
SOURCE /path/to/schema.sql;
```

**Option 2: From File**

```bash
# Save schema as schema.sql then run:
mysql -u root -p alpha_db < schema.sql
```

**Option 3: Using MySQL Workbench**

1. Open MySQL Workbench
2. Connect to your MySQL instance
3. Open schema.sql file
4. Click "Execute" (lightning bolt icon)

## 📝 Sample Data

### Insert Sample Data

```sql
-- ============================================
-- SAMPLE USERS
-- ============================================
-- Password: password123 (hashed with bcrypt)
INSERT INTO users (email, password, role, full_name, phone) VALUES
('admin@alpha.com', '$2b$10$YourHashedPasswordHere', 'admin', 'Admin User', '1234567890'),
('owner1@example.com', '$2b$10$YourHashedPasswordHere', 'owner', 'John Doe', '9876543210'),
('owner2@example.com', '$2b$10$YourHashedPasswordHere', 'owner', 'Jane Smith', '9876543211'),
('renter1@example.com', '$2b$10$YourHashedPasswordHere', 'renter', 'Mike Johnson', '9876543212'),
('renter2@example.com', '$2b$10$YourHashedPasswordHere', 'renter', 'Sarah Williams', '9876543213');

-- ============================================
-- SAMPLE SPACES
-- ============================================
INSERT INTO spaces (owner_id, title, description, address, city, latitude, longitude, price_per_hour, space_type, capacity, status) VALUES
(2, 'Downtown Parking Space', 'Secure covered parking near shopping district', '123 Main St, Downtown', 'Ranchi', 23.3441, 85.3096, 50.00, 'parking', 1, 'approved'),
(2, 'Airport Parking Lot', 'Long-term parking near airport', '456 Airport Rd', 'Ranchi', 23.3150, 85.3200, 40.00, 'parking', 50, 'approved'),
(3, 'Storage Unit - Medium', 'Climate controlled storage unit', '789 Storage Ave', 'Ranchi', 23.3600, 85.3300, 75.00, 'storage', 1, 'approved'),
(3, 'Warehouse Space', 'Large warehouse for commercial use', '321 Industrial Blvd', 'Ranchi', 23.3700, 85.3400, 200.00, 'warehouse', 1, 'pending');

-- ============================================
-- SAMPLE BOOKINGS
-- ============================================
INSERT INTO bookings (space_id, renter_id, start_time, end_time, total_hours, price_per_hour, total_amount, payment_status, booking_status) VALUES
(1, 4, '2026-02-01 09:00:00', '2026-02-01 17:00:00', 8.00, 50.00, 400.00, 'completed', 'completed'),
(2, 4, '2026-02-05 10:00:00', '2026-02-05 18:00:00', 8.00, 40.00, 320.00, 'completed', 'active'),
(3, 5, '2026-02-10 08:00:00', '2026-02-10 20:00:00', 12.00, 75.00, 900.00, 'pending', 'pending');

-- ============================================
-- SAMPLE REVIEWS
-- ============================================
INSERT INTO reviews (booking_id, space_id, reviewer_id, rating, review, is_verified) VALUES
(1, 1, 4, 5, 'Excellent parking space! Very secure and convenient location.', TRUE),
(2, 2, 4, 4, 'Good parking, but could be cleaner. Overall satisfied.', TRUE);
```

### Generate Test Data Script

For generating large amounts of test data, use this Node.js script:

```javascript
// generate-test-data.js
const mysql = require('mysql2/promise');
const bcrypt = require('bcrypt');

async function generateTestData() {
    const connection = await mysql.createConnection({
        host: 'localhost',
        user: 'root',
        password: 'your_password',
        database: 'alpha_db'
    });

    // Generate 100 test users
    for (let i = 1; i <= 100; i++) {
        const hashedPassword = await bcrypt.hash('password123', 10);
        const role = i % 3 === 0 ? 'owner' : 'renter';
        
        await connection.execute(
            'INSERT INTO users (email, password, role, full_name, phone) VALUES (?, ?, ?, ?, ?)',
            [`user${i}@test.com`, hashedPassword, role, `Test User ${i}`, `98765432${String(i).padStart(2, '0')}`]
        );
    }

    console.log('✅ Test data generated successfully');
    await connection.end();
}

generateTestData().catch(console.error);
```

Run: `node generate-test-data.js`

## 🔍 Database Management

### View Tables

```sql
-- Show all tables
SHOW TABLES;

-- Describe table structure
DESCRIBE users;
DESCRIBE spaces;
DESCRIBE bookings;
```

### Query Data

```sql
-- Get all users
SELECT * FROM users;

-- Get approved spaces
SELECT * FROM spaces WHERE status = 'approved';

-- Get active bookings
SELECT 
    b.id,
    u.full_name AS renter,
    s.title AS space,
    b.start_time,
    b.end_time,
    b.total_amount
FROM bookings b
JOIN users u ON b.renter_id = u.id
JOIN spaces s ON b.space_id = s.id
WHERE b.booking_status = 'active';
```

### Update Data

```sql
-- Approve pending space
UPDATE spaces SET status = 'approved' WHERE id = 1;

-- Cancel booking
UPDATE bookings SET booking_status = 'cancelled' WHERE id = 1;
```

### Indexes

```sql
-- Show indexes for a table
SHOW INDEX FROM bookings;

-- Create additional index
CREATE INDEX idx_user_created ON users(email, created_at);

-- Drop index
DROP INDEX idx_user_created ON users;
```

## 💾 Backup & Restore

### Backup Database

```bash
# Full database backup
mysqldump -u root -p alpha_db > alpha_db_backup_$(date +%Y%m%d).sql

# Backup specific tables
mysqldump -u root -p alpha_db users spaces bookings > alpha_db_partial.sql

# Backup with compression
mysqldump -u root -p alpha_db | gzip > alpha_db_backup.sql.gz
```

### Restore Database

```bash
# Restore from backup
mysql -u root -p alpha_db < alpha_db_backup_20260131.sql

# Restore from compressed backup
gunzip < alpha_db_backup.sql.gz | mysql -u root -p alpha_db
```

### Automated Backups

**Linux Cron Job:**

```bash
# Edit crontab
crontab -e

# Add daily backup at 2 AM
0 2 * * * mysqldump -u root -pYourPassword alpha_db > /backups/alpha_db_$(date +\%Y\%m\%d).sql
```

**Windows Task Scheduler:**

Create a batch file `backup.bat`:

```batch
@echo off
set timestamp=%date:~-4%%date:~-7,2%%date:~-10,2%
mysqldump -u root -pYourPassword alpha_db > C:\backups\alpha_db_%timestamp%.sql
```

Schedule in Task Scheduler to run daily.

## 🐛 Troubleshooting

### Cannot Connect to MySQL

```bash
# Check if MySQL is running
# Windows
sc query MySQL80

# Linux
sudo systemctl status mysql

# Start MySQL if not running
sudo systemctl start mysql
```

### Access Denied Error

```sql
-- Reset root password
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
FLUSH PRIVILEGES;

-- Grant permissions
GRANT ALL PRIVILEGES ON alpha_db.* TO 'root'@'localhost';
FLUSH PRIVILEGES;
```

### Table Already Exists

```sql
-- Drop table if exists
DROP TABLE IF EXISTS table_name;

-- Drop database and recreate
DROP DATABASE IF EXISTS alpha_db;
CREATE DATABASE alpha_db;
```

### Foreign Key Constraint Fails

```sql
-- Disable foreign key checks temporarily
SET FOREIGN_KEY_CHECKS = 0;

-- Run your queries

-- Re-enable foreign key checks
SET FOREIGN_KEY_CHECKS = 1;
```

### Slow Queries

```sql
-- Enable slow query log
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 2;

-- Check slow queries
SELECT * FROM mysql.slow_log;

-- Analyze query performance
EXPLAIN SELECT * FROM bookings WHERE renter_id = 1;
```

## 📊 Performance Optimization

### Optimize Tables

```sql
-- Optimize specific table
OPTIMIZE TABLE bookings;

-- Optimize all tables
OPTIMIZE TABLE users, spaces, bookings, payments;

-- Analyze table for better query planning
ANALYZE TABLE bookings;
```

### Connection Pooling

Already implemented in backend `db.js`:

```javascript
const pool = mysql.createPool({
    host: process.env.DB_HOST,
    user: process.env.DB_USER,
    password: process.env.DB_PASS,
    database: process.env.DB_NAME,
    waitForConnections: true,
    connectionLimit: 10,
    queueLimit: 0
});
```

## 🔒 Security Best Practices

1. **Strong Passwords**
   - Use complex passwords for MySQL users
   - Change default root password

2. **Least Privilege**
   - Create specific users for application
   - Grant only necessary permissions

3. **Regular Backups**
   - Automate daily backups
   - Store backups securely off-site

4. **SQL Injection Prevention**
   - Use prepared statements (already implemented)
   - Never concatenate user input into queries

5. **Encryption**
   - Enable SSL for connections
   - Encrypt sensitive data at rest

## 📚 Additional Resources

- [MySQL Documentation](https://dev.mysql.com/doc/)
- [MySQL Workbench Guide](https://dev.mysql.com/doc/workbench/en/)
- [Database Design Best Practices](https://dev.mysql.com/doc/refman/8.0/en/optimization.html)

---

**Next Steps:**
- ✅ Database setup complete
- 🔙 Configure [Backend Connection](./BACKEND_SETUP.md)
- 🚀 Start the application
