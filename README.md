# 🚀 Alpha - Smart Space Management Platform

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![React](https://img.shields.io/badge/React-19.2.0-blue.svg)](https://reactjs.org/)
[![Node.js](https://img.shields.io/badge/Node.js-Express-green.svg)](https://nodejs.org/)
[![MySQL](https://img.shields.io/badge/MySQL-Database-orange.svg)](https://www.mysql.com/)
[![Python](https://img.shields.io/badge/Python-ML%20Service-blue.svg)](https://www.python.org/)

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [ML Service](#ml-service)
- [Environment Variables](#environment-variables)
- [Running the Application](#running-the-application)
- [Database Schema](#database-schema)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

**Alpha** is an intelligent space management platform that revolutionizes how businesses and individuals manage, rent, and optimize parking/storage spaces. Built with cutting-edge technologies, Alpha combines real-time booking, AI-powered chatbot assistance, image validation, and smart payment processing to deliver a seamless user experience.

### 🏆 Hackathon Highlights

- **Real-time Space Management**: Dynamic availability tracking and instant booking
- **AI-Powered Features**: ML-based image validation and intelligent chatbot
- **Secure Payment Integration**: Stripe integration with webhook verification
- **Multi-role System**: Separate dashboards for space owners, renters, and administrators
- **Interactive Maps**: Leaflet-based mapping with routing capabilities
- **Responsive Design**: Modern UI built with React and Tailwind CSS

## ✨ Features

### For Space Owners (Manage Space)
- 🏢 **Dashboard**: Comprehensive view of all managed spaces
- 📊 **Analytics**: Real-time rental statistics and revenue tracking
- 📸 **Image Upload**: Secure space image uploads with validation
- 📅 **Availability Management**: Control space availability with calendar interface
- 💰 **Revenue Tracking**: Monitor earnings and booking history

### For Renters (Rent Space)
- 🔍 **Smart Search**: Find available spaces with map-based interface
- 🗺️ **Route Planning**: Integrated navigation to selected spaces
- 💳 **Secure Payments**: Stripe-powered payment processing
- 📱 **Booking History**: Track all past and current bookings
- 🔔 **Real-time Updates**: Instant booking confirmations

### For Administrators
- 👥 **User Management**: Oversee all users and space owners
- ✅ **Approval System**: Review and approve new space listings
- 📈 **Platform Analytics**: Monitor platform-wide metrics
- 🛡️ **Security Controls**: Manage access and permissions

### Universal Features
- 🤖 **AI Chatbot**: 24/7 intelligent customer support
- 🔐 **Secure Authentication**: Role-based access control
- 📱 **Responsive Design**: Works seamlessly on all devices
- 🌐 **Real-time Updates**: Live data synchronization
- 🎨 **Modern UI/UX**: Intuitive and beautiful interface

## 🛠️ Tech Stack

### Frontend
- **Framework**: React 19.2.0
- **Routing**: React Router DOM 7.13.0
- **Styling**: Tailwind CSS 4.1.18
- **Build Tool**: Vite 7.2.4
- **Maps**: Leaflet 1.9.4 with Routing Machine
- **Payments**: Stripe.js 8.7.0
- **UI Components**: React Icons, React Hot Toast
- **Timeline**: React Calendar Timeline
- **Date/Time**: Moment.js

### Backend
- **Runtime**: Node.js
- **Framework**: Express 5.2.1
- **Database**: MySQL 2 (with Promise support)
- **Environment**: dotenv 17.2.3
- **Development**: Nodemon 3.1.11
- **File Upload**: Multer (for image handling)

### ML Service
- **Framework**: FastAPI 0.104.1
- **Server**: Uvicorn 0.24.0
- **Computer Vision**: OpenCV 4.8.1.78
- **Numerical Computing**: NumPy 1.24.3
- **Machine Learning**: Scikit-learn 1.3.2
- **Visualization**: Matplotlib 3.8.2

### Database
- **Primary Database**: MySQL
- **Connection**: mysql2/promise
- **Connection Pooling**: Built-in pool management

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         Frontend (React)                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Manage     │  │     Rent     │  │    Admin     │      │
│  │    Space     │  │    Space     │  │   Dashboard  │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    Backend (Express/Node.js)                 │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              REST API Controllers                     │   │
│  │  • Authentication  • Booking  • Payment  • Upload    │   │
│  └──────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────┐   │
│  │                    Services Layer                     │   │
│  │  • Image Validation  • Chatbot  • Business Logic     │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
           │                              │
           ▼                              ▼
┌────────────────────┐         ┌────────────────────┐
│   MySQL Database   │         │   ML Service       │
│   • Users          │         │   (FastAPI/Python) │
│   • Spaces         │         │   • Image Analysis │
│   • Bookings       │         │   • Validation     │
│   • Transactions   │         └────────────────────┘
└────────────────────┘
           │
           ▼
┌────────────────────┐
│  Stripe Payment    │
│  Gateway           │
└────────────────────┘
```

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v18.0.0 or higher)
- **npm** (v9.0.0 or higher) or **yarn**
- **MySQL** (v8.0 or higher)
- **Python** (v3.9 or higher) - for ML service
- **Git** - for version control
- **Stripe Account** - for payment processing

## 🚀 Installation

### Quick Start

1. **Clone the Repository**
   ```bash
   git clone https://github.com/Quasar-x-AI-2026/Alpha.git
   cd Alpha
   ```

2. **Database Setup**
   ```bash
   # Follow DATABASE_SETUP.md for detailed instructions
   mysql -u root -p < database/schema.sql
   ```

3. **Backend Setup**
   ```bash
   cd back-end
   npm install
   cp .env.example .env
   # Configure your .env file with database credentials
   npm start
   ```

4. **Frontend Setup**
   ```bash
   cd ../front-end
   npm install
   npm run dev
   ```

5. **ML Service Setup** (Optional)
   ```bash
   cd ../back-end/ml-service
   pip install -r requirements.txt
   python app.py
   ```

### Detailed Setup Instructions

For detailed setup instructions, please refer to:
- 📘 [Frontend Setup Guide](./FRONTEND_SETUP.md)
- 📗 [Backend Setup Guide](./BACKEND_SETUP.md)
- 📙 [Database Setup Guide](./DATABASE_SETUP.md)
- 📕 [ML Service Setup Guide](./ML_SERVICE_SETUP.md)

## 📁 Project Structure

```
Alpha/
├── back-end/                    # Backend server
│   ├── app/
│   │   ├── controller/         # Route controllers
│   │   │   ├── admin.controller.js
│   │   │   ├── chatbot.controller.js
│   │   │   ├── fileUpload.controller.js
│   │   │   ├── manageSpace.controller.js
│   │   │   └── rentSpace.controller.js
│   │   ├── model/              # Database models
│   │   │   └── db.js
│   │   ├── router/             # API routes
│   │   │   └── router.js
│   │   └── services/           # Business logic
│   │       └── imageValidation.service.js
│   ├── ml-service/             # Python ML service
│   │   ├── app.py
│   │   ├── model.p
│   │   ├── requirements.txt
│   │   └── utils.py
│   ├── public/                 # Static files/uploads
│   ├── .env                    # Environment variables
│   ├── .gitignore
│   └── package.json
│
├── front-end/                   # React frontend
│   ├── src/
│   │   ├── components/         # React components
│   │   │   ├── admin/         # Admin dashboard
│   │   │   ├── chatbot/       # Chatbot interface
│   │   │   ├── form/          # Form components
│   │   │   ├── home/          # Landing page
│   │   │   ├── manage-space/  # Space owner dashboard
│   │   │   └── rent-space/    # Renter dashboard
│   │   ├── pages/             # Page components
│   │   ├── util/              # Utilities
│   │   │   ├── fetchData.js
│   │   │   └── GlobalContextComponent.jsx
│   │   ├── App.jsx            # Main app component
│   │   ├── index.jsx          # Entry point
│   │   └── index.css          # Global styles
│   ├── public/                # Static assets
│   ├── vite.config.js         # Vite configuration
│   ├── tailwind.config.js     # Tailwind configuration
│   └── package.json
│
├── README.md                    # This file
├── FRONTEND_SETUP.md           # Frontend setup guide
├── BACKEND_SETUP.md            # Backend setup guide
├── DATABASE_SETUP.md           # Database setup guide
└── ML_SERVICE_SETUP.md         # ML service setup guide
```

## 🔌 API Documentation

### Authentication Endpoints

#### Manage Space Login
```http
POST /api/manage-space/login
Content-Type: application/json

{
  "email": "owner@example.com",
  "password": "password123"
}
```

#### Rent Space Login
```http
POST /api/rent-space/login
Content-Type: application/json

{
  "email": "renter@example.com",
  "password": "password123"
}
```

### Booking Endpoints

#### Get Available Spaces
```http
POST /api/rent-space/get-marker
Content-Type: application/json

{
  "latitude": 23.3441,
  "longitude": 85.3096
}
```

#### Book Space
```http
POST /api/rent-space/book-ticket
Content-Type: application/json

{
  "spaceId": 123,
  "userId": 456,
  "startTime": "2026-02-01T10:00:00",
  "endTime": "2026-02-01T18:00:00"
}
```

#### Create Payment Session
```http
POST /create-checkout-session
Content-Type: application/json

{
  "amount": 2000,
  "bookingId": 789
}
```

### File Upload

#### Upload Space Image
```http
POST /api/upload
Content-Type: multipart/form-data

file: [image file]
userId: 123
```

### Chatbot

#### Chat with AI
```http
POST /chat
Content-Type: application/json

{
  "message": "What are the available parking spaces near me?",
  "userId": 123
}
```

For complete API documentation, see [API_REFERENCE.md](./API_REFERENCE.md)

## 🤖 ML Service

The ML service provides image validation and analysis capabilities:

### Features
- **Image Validation**: Verify uploaded space images
- **Quality Check**: Ensure images meet minimum quality standards
- **Content Detection**: Identify inappropriate or invalid content
- **Dimension Analysis**: Check image dimensions and aspect ratios

### Endpoints

```http
POST http://localhost:8000/validate-image
Content-Type: multipart/form-data

file: [image file]
```

## 🔐 Environment Variables

### Backend (.env)

```env
# Database Configuration
DB_HOST=localhost
DB_USER=root
DB_PASS=your_password
DB_NAME=alpha_db

# Server Configuration
PORT=3000
NODE_ENV=development

# Stripe Configuration
STRIPE_SECRET_KEY=sk_test_your_stripe_secret_key
STRIPE_WEBHOOK_SECRET=whsec_your_webhook_secret

# ML Service
ML_SERVICE_URL=http://localhost:8000

# JWT Secret (for authentication)
JWT_SECRET=your_jwt_secret_key_here

# Frontend URL
FRONTEND_URL=http://localhost:5173
```

### Frontend (.env)

```env
# API Configuration
VITE_API_URL=http://localhost:3000
VITE_STRIPE_PUBLIC_KEY=pk_test_your_stripe_public_key

# Map Configuration
VITE_MAP_API_KEY=your_map_api_key
```

## 🏃‍♂️ Running the Application

### Development Mode

1. **Start MySQL Server**
   ```bash
   # On Windows (XAMPP)
   # Start MySQL from XAMPP Control Panel
   
   # On Linux/Mac
   sudo systemctl start mysql
   ```

2. **Start Backend Server**
   ```bash
   cd back-end
   npm run dev
   # Server runs on http://localhost:3000
   ```

3. **Start Frontend Development Server**
   ```bash
   cd front-end
   npm run dev
   # Server runs on http://localhost:5173
   ```

4. **Start ML Service** (Optional)
   ```bash
   cd back-end/ml-service
   python app.py
   # Service runs on http://localhost:8000
   ```

### Production Build

```bash
# Build Frontend
cd front-end
npm run build

# Start Backend in Production
cd ../back-end
NODE_ENV=production node index.js
```

## 🗄️ Database Schema

### Users Table
```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    role ENUM('owner', 'renter', 'admin') NOT NULL,
    full_name VARCHAR(255) NOT NULL,
    phone VARCHAR(20),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### Spaces Table
```sql
CREATE TABLE spaces (
    id INT AUTO_INCREMENT PRIMARY KEY,
    owner_id INT NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    latitude DECIMAL(10, 8) NOT NULL,
    longitude DECIMAL(11, 8) NOT NULL,
    price_per_hour DECIMAL(10, 2) NOT NULL,
    status ENUM('pending', 'approved', 'rejected') DEFAULT 'pending',
    images JSON,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (owner_id) REFERENCES users(id)
);
```

### Bookings Table
```sql
CREATE TABLE bookings (
    id INT AUTO_INCREMENT PRIMARY KEY,
    space_id INT NOT NULL,
    renter_id INT NOT NULL,
    start_time DATETIME NOT NULL,
    end_time DATETIME NOT NULL,
    total_amount DECIMAL(10, 2) NOT NULL,
    payment_status ENUM('pending', 'completed', 'failed') DEFAULT 'pending',
    booking_status ENUM('active', 'completed', 'cancelled') DEFAULT 'active',
    stripe_session_id VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (space_id) REFERENCES spaces(id),
    FOREIGN KEY (renter_id) REFERENCES users(id)
);
```

For complete database schema, see [DATABASE_SETUP.md](./DATABASE_SETUP.md)

## 🧪 Testing

```bash
# Run Backend Tests
cd back-end
npm test

# Run Frontend Tests
cd front-end
npm test
```

## 📊 Features Roadmap

- [ ] Mobile Application (React Native)
- [ ] Real-time Chat between owners and renters
- [ ] Advanced Analytics Dashboard
- [ ] Multi-language Support
- [ ] Push Notifications
- [ ] Integration with Google Maps API
- [ ] QR Code Check-in/Check-out
- [ ] Automated Pricing based on Demand
- [ ] Review and Rating System

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👥 Team

- **Project Lead**: Quasar-x-AI-2026
- **Repository**: [https://github.com/Quasar-x-AI-2026/Alpha](https://github.com/Quasar-x-AI-2026/Alpha)

## 📞 Support

For support, email support@alpha-platform.com or join our Slack channel.

## 🙏 Acknowledgments

- React Team for the amazing framework
- Express.js for the robust backend framework
- Stripe for seamless payment integration
- Leaflet for interactive maps
- All contributors who helped build this project

---

**Built with ❤️ for Hackathon 2026**
