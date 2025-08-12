# 🍽️ Food Rescue API

A secure JWT-based REST API that connects restaurants with surplus food to NGOs and food banks, helping reduce food waste and fight hunger.

![PHP](https://img.shields.io/badge/PHP-8.0+-777BB4?style=flat-square&logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-5.7+-4479A1?style=flat-square&logo=mysql&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-Authentication-000000?style=flat-square&logo=jsonwebtokens&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

## 🌟 Features

### Authentication & Security
- 🔐 **JWT Authentication** with token-based authorization
- 🛡️ **Bcrypt Password Hashing** for secure password storage
- 🔑 **Role-based Access Control** (Restaurant/NGO)
- 🚫 **Protected Endpoints** requiring valid JWT tokens
- 🌐 **CORS Support** for cross-origin requests

### Food Management System
- 📝 **Create Food Listings** - Restaurants can post surplus food
- 📋 **Browse Available Food** - NGOs can view all listings
- 🏪 **Organization Profiles** - Complete user management
- ⏰ **Expiry Date Tracking** - Food freshness monitoring
- 📍 **Location-based Listings** - Geographic food mapping

### API Architecture
- 🏗️ **RESTful Design** with proper HTTP methods
- 📊 **JSON Responses** with consistent error handling
- 🔄 **Environment-based Configuration** for deployment flexibility
- 📱 **Mobile App Ready** - Compatible with Flutter/React Native

## 🚀 Quick Start

### Prerequisites

- **XAMPP** or **WAMP** (Apache + MySQL + PHP 8.0+)
- **Composer** (PHP package manager)
- **Git** (version control)

### Installation

1. **Clone the repository**

git clone https://github.com/arnavbhatia2003/food-rescue-api.git
cd food-rescue-api

2. **Install dependencies**

composer install

3. **Database setup**
- Open phpMyAdmin: http://localhost/phpmyadmin
- Create database: ood_rescue
- Import schema: database/schema.sql

4. **Environment configuration**

cp .env.example .env
Edit .env with your database credentials and JWT secrets

5. **Start the server**
- Place project in: C:\xampp\htdocs\food-rescue-api
- Start XAMPP (Apache + MySQL)
- Visit: http://localhost/food-rescue-api

## 📖 API Documentation

### Base URL

http://localhost/food-rescue-api

### Authentication

#### Login

POST /auth/login.php
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password"
}

**Response:**

{
  "success": true,
  "message": "Login ok",
  "token": "eyJ0eXAiOiJKV1Q...",
  "user": {
    "id": "1",
    "username": "restaurant1",
    "email": "user@example.com",
    "user_type": "restaurant",
    "organization_name": "Fresh Food Restaurant",
    "contact_person": "John Doe"
  }
}

### Food Listings

#### Create Listing (Protected)

POST /food/add_listing.php
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json

{
  "title": "Fresh Bread",
  "description": "Unsold artisan bread from bakery",
  "quantity": "20 loaves",
  "location": "Downtown Bakery, Main St",
  "expiry_date": "2025-08-13"
}

**Response:**

{
  "success": true,
  "listing_id": "1"
}

#### Get All Listings (Public)

GET /food/get_listings.php

**Response:**

[
  {
    "id": "1",
    "user_id": "1",
    "title": "Fresh Bread",
    "description": "Unsold artisan bread from bakery",
    "quantity": "20 loaves",
    "location": "Downtown Bakery, Main St",
    "expiry_date": "2025-08-13",
    "status": "available",
    "created_at": "2025-08-12 10:30:00"
  }
]

## 🏗️ Project Structure

food-rescue-api/
├── auth/
│ └── login.php # JWT authentication endpoint
├── config/
│ ├── database.php # Database connection
│ └── auth_middleware.php # JWT validation middleware
├── food/
│ ├── add_listing.php # Create food listings (protected)
│ └── get_listings.php # Fetch all listings (public)
├── database/
│ └── schema.sql # Database schema
├── vendor/ # Composer dependencies
├── .env.example # Environment template
├── .gitignore # Git ignore rules
├── composer.json # PHP dependencies
└── README.md # This file

## 🗄️ Database Schema

### Users Table

CREATE TABLE users (
  id INT PRIMARY KEY AUTO_INCREMENT,
  username VARCHAR(50) UNIQUE NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  user_type ENUM('restaurant','ngo') NOT NULL,
  organization_name VARCHAR(100) NOT NULL,
  contact_person VARCHAR(100) NOT NULL,
  phone VARCHAR(20) NOT NULL,
  address TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

### Food Listings Table

CREATE TABLE food_listings (
  id INT PRIMARY KEY AUTO_INCREMENT,
  user_id INT NOT NULL,
  title VARCHAR(200) NOT NULL,
  description TEXT,
  quantity VARCHAR(100),
  location VARCHAR(200) NOT NULL,
  expiry_date DATE NOT NULL,
  status ENUM('available','claimed','expired') DEFAULT 'available',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id)
);

## 🔧 Configuration

### Environment Variables (.env)

Database Configuration

DB_HOST=localhost
DB_NAME=food_rescue
DB_USER=root
DB_PASS=
JWT Configuration

JWT_SECRET=your-super-secret-jwt-key-change-this
JWT_ISS=food-rescue.local
JWT_AUD=food-rescue.local
JWT_TTL_SECONDS=3600
CORS Configuration

CORS_ORIGIN=*

## 🧪 Testing

### Using PowerShell

**1. Login and get JWT token:**

$response = Invoke-RestMethod -Uri "http://localhost/food-rescue-api/auth/login.php" -Method POST -ContentType "application/json" -Body '{\"email\":\"user@example.com\",\"password\":\"password\"}'
$TOKEN = ï»¿ï»¿ï»¿{"success":true,"message":"Login ok","token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJmb29kLXJlc2N1ZS5sb2NhbCIsImF1ZCI6ImZvb2QtcmVzY3VlLmxvY2FsIiwiaWF0IjoxNzU0OTc2NTE1LCJuYmYiOjE3NTQ5NzY1MTUsImV4cCI6MTc1NDk4MDExNSwic3ViIjoiMSIsInJvbGUiOiJyZXN0YXVyYW50IiwiZW1haWwiOiJ1c2VyMkBnbWFpbC5jb20ifQ.LGyP7FX5DSYk0xW3LflEImudWq7TSfyuwc10gbkHChs","user":{"id":1,"name":"Test User","email":"user2@gmail.com","user_type":"restaurant"}}.token

**2. Create protected food listing:**

$listing = Invoke-RestMethod -Uri "http://localhost/food-rescue-api/food/add_listing.php" -Method POST -ContentType "application/json" -Headers @{"Authorization"="Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJmb29kLXJlc2N1ZS5sb2NhbCIsImF1ZCI6ImZvb2QtcmVzY3VlLmxvY2FsIiwiaWF0IjoxNzU0OTc4OTU1LCJuYmYiOjE3NTQ5Nzg5NTUsImV4cCI6MTc1NDk4MjU1NSwic3ViIjoiMyIsInJvbGUiOiJyZXN0YXVyYW50IiwiZW1haWwiOiJ1c2VyMjAwM0BnbWFpbC5jb20ifQ.3b01JkOva5MpGnbZClENZ9llVGQ7ggjFA14k3cYoWPE"} -Body '{\"title\":\"Fresh Vegetables\",\"description\":\"Surplus produce\",\"quantity\":\"10 kg\",\"location\":\"Green Market\",\"expiry_date\":\"2025-08-13\"}'

**3. Fetch all listings:**

$listings = Invoke-RestMethod -Uri "http://localhost/food-rescue-api/food/get_listings.php"

### Using cURL

**1. Login:**

curl -X POST http://localhost/food-rescue-api/auth/login.php \
-H "Content-Type: application/json" \
-d '{"email":"user@example.com","password":"password"}'

**2. Create listing:**

curl -X POST http://localhost/food-rescue-api/food/add_listing.php \
-H "Content-Type: application/json" \
-H "Authorization: Bearer YOUR_JWT_TOKEN" \
-d '{"title":"Fresh Bread","description":"Surplus bread","quantity":"10 loaves","location":"Bakery Street","expiry_date":"2025-08-13"}'

## 🛡️ Security Features

- **Password Security:** All passwords stored using bcrypt hashing
- **JWT Tokens:** Secure token-based authentication with expiration
- **Input Validation:** SQL injection prevention with prepared statements
- **CORS Protection:** Configurable cross-origin resource sharing
- **Environment Secrets:** Sensitive data stored in environment variables
- **Role-based Access:** Different permissions for restaurants and NGOs

## 🌐 CORS Configuration

The API supports Cross-Origin Resource Sharing (CORS) for integration with web and mobile applications:

header("Access-Control-Allow-Origin: *");
header("Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS");
header("Access-Control-Allow-Headers: Content-Type, Authorization");

## 📱 Mobile App Integration

This API is designed to work seamlessly with mobile applications built using:
- **Flutter** (Dart)
- **React Native** (JavaScript/TypeScript)
- **Native Android** (Java/Kotlin)
- **Native iOS** (Swift/Objective-C)

Example Flutter integration:

// Login request
final response = await http.post(
  Uri.parse('http://your-domain.com/food-rescue-api/auth/login.php'),
  headers: {'Content-Type': 'application/json'},
  body: jsonEncode({'email': email, 'password': password}),
);

## 🚀 Deployment

### Production Checklist

- [ ] Change JWT_SECRET to a strong, unique key
- [ ] Update database credentials in .env
- [ ] Configure proper CORS origins
- [ ] Enable HTTPS/SSL certificates
- [ ] Set up database backups
- [ ] Configure error logging
- [ ] Implement rate limiting
- [ ] Set up monitoring and alerts

### Hosting Options

- **Shared Hosting:** Most PHP hosting providers
- **VPS/Cloud:** DigitalOcean, AWS, Google Cloud
- **Platform as a Service:** Heroku, Railway
- **Dedicated Server:** For high-traffic applications

## 🤝 Contributing

We welcome contributions to the Food Rescue API! Here's how you can help:

1. **Fork the repository**

git clone https://github.com/arnavbhatia2003/food-rescue-api.git

2. **Create your feature branch**

git checkout -b feature/amazing-feature

3. **Make your changes**
- Follow PSR-12 coding standards
- Add tests for new features
- Update documentation

4. **Commit your changes**

git commit -m 'Add amazing feature'

5. **Push to the branch**

git push origin feature/amazing-feature

6. **Open a Pull Request**
- Visit: https://github.com/arnavbhatia2003/food-rescue-api/pulls
- Provide a clear description of your changes

### Development Guidelines

- Follow **PSR-12** PHP coding standards
- Write **meaningful commit messages**
- Add **PHPDoc comments** to all functions
- Include **unit tests** for new features
- Update **API documentation** for endpoint changes

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

MIT License

Copyright (c) 2025 Arnav Bhatia

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

## 🎯 Roadmap

### Current Version (v1.0)
- ✅ JWT Authentication
- ✅ Food Listings CRUD
- ✅ User Management
- ✅ Security Implementation

### Upcoming Features (v2.0)
- 🔄 User Registration API
- 🔄 Food Claiming System
- 🔄 Search and Filtering
- 🔄 Image Upload Support
- 🔄 Email Notifications
- 🔄 Analytics Dashboard

### Future Enhancements (v3.0)
- 🔮 Real-time Notifications
- 🔮 Geographic Proximity Matching
- 🔮 Rating and Review System
- 🔮 Admin Panel
- 🔮 API Rate Limiting
- 🔮 Multi-language Support

## 🙏 Acknowledgments

- **Firebase JWT PHP** - Robust JWT implementation for PHP
- **Composer** - Dependency management made simple
- **XAMPP** - Complete local development environment
- **MySQL** - Reliable database management system
- **PHP Community** - For excellent documentation and support

## 📞 Support

Need help with the Food Rescue API? Here are your options:

- **📧 Email:** arnavbhatia2003@gmail.com
- **🐛 Issues:** [GitHub Issues](https://github.com/arnavbhatia2003/food-rescue-api/issues)
- **💬 Discussions:** [GitHub Discussions](https://github.com/arnavbhatia2003/food-rescue-api/discussions)
- **📖 Wiki:** [Project Wiki](https://github.com/arnavbhatia2003/food-rescue-api/wiki)

## 📊 Project Stats

- **Language:** PHP 8.0+
- **Database:** MySQL 5.7+
- **Authentication:** JWT (JSON Web Tokens)
- **Security:** Bcrypt Password Hashing
- **Architecture:** RESTful API
- **License:** MIT
- **Status:** Active Development

---

**Built with ❤️ for reducing food waste and fighting hunger**

*Making a difference, one meal at a time.*
