# 🔧 Backend Setup Guide

Complete guide to set up the Alpha backend server.

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Environment Configuration](#environment-configuration)
- [Database Connection](#database-connection)
- [Running the Server](#running-the-server)
- [API Testing](#api-testing)
- [Troubleshooting](#troubleshooting)

## 🎯 Prerequisites

Before setting up the backend, ensure you have:

- **Node.js** (v18.0.0 or higher)
- **npm** (v9.0.0 or higher) or **yarn**
- **MySQL** (v8.0 or higher) - Must be installed and running
- **Git** - For cloning the repository
- **Stripe Account** - For payment processing
- **Text Editor** - VS Code recommended

### Check Prerequisites

```bash
# Check Node.js version
node --version
# Should output: v18.0.0 or higher

# Check npm version
npm --version
# Should output: v9.0.0 or higher

# Check MySQL version
mysql --version
# Should output: mysql Ver 8.0.x or higher
```

## 📦 Installation

### Step 1: Navigate to Backend Directory

```bash
cd Alpha/back-end
```

### Step 2: Install Dependencies

```bash
npm install
```

This will install:
- **express** (v5.2.1) - Web framework
- **dotenv** (v17.2.3) - Environment variable management
- **nodemon** (v3.1.11) - Auto-restart during development
- **mysql2** - MySQL database driver with Promise support
- **multer** - File upload middleware
- **stripe** - Payment processing
- **bcrypt** - Password hashing
- **jsonwebtoken** - JWT authentication

### Step 3: Verify Installation

```bash
# Check if node_modules folder was created
ls node_modules

# Verify package.json
cat package.json
```

## 🔐 Environment Configuration

### Step 1: Create .env File

```bash
# Create .env file from template
touch .env

# Or on Windows PowerShell
New-Item .env -ItemType File
```

### Step 2: Configure Environment Variables

Open `.env` file and add the following configuration:

```env
# ============================================
# DATABASE CONFIGURATION
# ============================================
DB_HOST=localhost
DB_USER=root
DB_PASS=your_mysql_password
DB_NAME=alpha_db
DB_PORT=3306

# ============================================
# SERVER CONFIGURATION
# ============================================
PORT=3000
NODE_ENV=development
HOST=localhost

# ============================================
# STRIPE PAYMENT CONFIGURATION
# ============================================
# Get these from: https://dashboard.stripe.com/apikeys
STRIPE_SECRET_KEY=sk_test_51xxxxxxxxxxxxxxxxxxxxx
STRIPE_PUBLISHABLE_KEY=pk_test_51xxxxxxxxxxxxxxxxxxxxx
STRIPE_WEBHOOK_SECRET=whsec_xxxxxxxxxxxxxxxxxxxxx

# ============================================
# JWT AUTHENTICATION
# ============================================
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
JWT_EXPIRY=24h

# ============================================
# FILE UPLOAD CONFIGURATION
# ============================================
UPLOAD_DIR=./public
MAX_FILE_SIZE=5242880
# 5MB = 5 * 1024 * 1024 bytes

# ============================================
# ML SERVICE CONFIGURATION
# ============================================
ML_SERVICE_URL=http://localhost:8000
ML_SERVICE_ENABLED=true

# ============================================
# FRONTEND CONFIGURATION
# ============================================
FRONTEND_URL=http://localhost:5173
CORS_ORIGIN=http://localhost:5173

# ============================================
# CHATBOT CONFIGURATION
# ============================================
CHATBOT_API_KEY=your_chatbot_api_key
CHATBOT_MODEL=gpt-3.5-turbo

# ============================================
# EMAIL CONFIGURATION (Optional)
# ============================================
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_app_password
EMAIL_FROM=noreply@alpha-platform.com

# ============================================
# LOGGING
# ============================================
LOG_LEVEL=debug
LOG_FILE=./logs/app.log
```

### Step 3: Security Considerations

⚠️ **IMPORTANT**: Never commit your `.env` file to version control!

```bash
# Verify .env is in .gitignore
cat .gitignore | grep .env

# Add to .gitignore if not present
echo ".env" >> .gitignore
```

## 🗄️ Database Connection

### Step 1: Ensure MySQL is Running

```bash
# On Windows (XAMPP)
# Open XAMPP Control Panel and start MySQL

# On Linux
sudo systemctl status mysql
sudo systemctl start mysql

# On macOS
brew services start mysql
```

### Step 2: Test Database Connection

Create a test file `test-db.js`:

```javascript
require('dotenv').config();
const mysql = require('mysql2/promise');

async function testConnection() {
    try {
        const connection = await mysql.createConnection({
            host: process.env.DB_HOST,
            user: process.env.DB_USER,
            password: process.env.DB_PASS,
            database: process.env.DB_NAME
        });
        
        console.log('✅ Database connection successful!');
        await connection.end();
    } catch (error) {
        console.error('❌ Database connection failed:', error.message);
    }
}

testConnection();
```

Run the test:

```bash
node test-db.js
```

### Step 3: Set Up Database Schema

Follow the [DATABASE_SETUP.md](./DATABASE_SETUP.md) guide to create the database schema.

## 🚀 Running the Server

### Development Mode (with auto-restart)

```bash
npm run dev
```

Or using nodemon directly:

```bash
npx nodemon index.js
```

### Production Mode

```bash
npm start
```

Or:

```bash
NODE_ENV=production node index.js
```

### Expected Output

```
🚀 Server is running on port 3000
📊 Environment: development
🗄️  Database connected successfully
✅ All services initialized
```

### Verify Server is Running

Open your browser or use curl:

```bash
# Test health endpoint
curl http://localhost:3000/health

# Or in browser
http://localhost:3000/health
```

## 🧪 API Testing

### Using cURL

#### Test Authentication

```bash
# Register new user
curl -X POST http://localhost:3000/api/manage-space/add-user \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123",
    "fullName": "Test User",
    "phone": "1234567890"
  }'

# Login
curl -X POST http://localhost:3000/api/manage-space/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123"
  }'
```

#### Test File Upload

```bash
curl -X POST http://localhost:3000/api/upload \
  -F "file=@/path/to/image.jpg" \
  -F "userId=1"
```

### Using Postman

1. **Import Collection**
   - Download [Alpha API Collection](./postman/Alpha-API.postman_collection.json)
   - Import into Postman

2. **Set Environment Variables**
   - Create new environment
   - Add variables:
     - `base_url`: http://localhost:3000
     - `token`: (will be set after login)

3. **Test Endpoints**
   - Start with authentication endpoints
   - Use the token for protected routes

### Using Thunder Client (VS Code Extension)

1. Install Thunder Client extension
2. Import `thunder-client-collection.json`
3. Run tests directly in VS Code

## 🐛 Troubleshooting

### Port Already in Use

```bash
# Error: EADDRINUSE: address already in use :::3000

# Solution 1: Kill process using port 3000
# On Windows
netstat -ano | findstr :3000
taskkill /PID <PID> /F

# On Linux/Mac
lsof -ti:3000 | xargs kill -9

# Solution 2: Change port in .env
PORT=3001
```

### Database Connection Errors

```bash
# Error: ER_ACCESS_DENIED_ERROR

# Solution: Check credentials in .env
# Verify MySQL user has proper permissions

# Grant permissions in MySQL
mysql -u root -p
GRANT ALL PRIVILEGES ON alpha_db.* TO 'root'@'localhost';
FLUSH PRIVILEGES;
```

### Module Not Found Errors

```bash
# Error: Cannot find module 'express'

# Solution: Reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

### Stripe Webhook Errors

```bash
# Error: Webhook signature verification failed

# Solution 1: Use Stripe CLI for local testing
stripe listen --forward-to localhost:3000/web-hook

# Solution 2: Update STRIPE_WEBHOOK_SECRET in .env
# Get from Stripe CLI output or Dashboard
```

### File Upload Errors

```bash
# Error: ENOENT: no such file or directory

# Solution: Create upload directory
mkdir -p public/uploads
chmod 755 public/uploads
```

### ML Service Connection Errors

```bash
# Error: connect ECONNREFUSED 127.0.0.1:8000

# Solution: Start ML service first
cd ml-service
python app.py

# Or disable ML service in .env
ML_SERVICE_ENABLED=false
```

## 📊 Monitoring & Logs

### View Logs

```bash
# Real-time logs
tail -f logs/app.log

# Error logs only
tail -f logs/error.log

# Using npm script (if configured)
npm run logs
```

### Debug Mode

```bash
# Enable debug mode
DEBUG=* npm run dev

# Debug specific modules
DEBUG=express:* npm run dev
```

## 🔒 Security Best Practices

1. **Environment Variables**
   - Never commit `.env` file
   - Use strong, random JWT_SECRET
   - Rotate API keys regularly

2. **Database Security**
   - Use strong MySQL passwords
   - Create dedicated database user (not root)
   - Limit user permissions

3. **API Security**
   - Implement rate limiting
   - Use HTTPS in production
   - Validate all inputs
   - Sanitize user data

4. **File Uploads**
   - Validate file types
   - Limit file sizes
   - Scan for malware
   - Store outside web root

## 🚀 Deployment

### Production Checklist

- [ ] Set `NODE_ENV=production`
- [ ] Use production database
- [ ] Enable HTTPS
- [ ] Set up reverse proxy (nginx)
- [ ] Configure firewall
- [ ] Set up monitoring
- [ ] Configure backup strategy
- [ ] Enable logging
- [ ] Set up error tracking (Sentry)
- [ ] Configure CORS properly

### Deploy to Heroku

```bash
# Login to Heroku
heroku login

# Create new app
heroku create alpha-backend

# Set environment variables
heroku config:set DB_HOST=your_db_host
heroku config:set DB_USER=your_db_user
# ... set all env variables

# Deploy
git push heroku main

# View logs
heroku logs --tail
```

### Deploy to AWS EC2

```bash
# SSH into EC2 instance
ssh -i your-key.pem ubuntu@your-ec2-ip

# Clone repository
git clone https://github.com/Quasar-x-AI-2026/Alpha.git
cd Alpha/back-end

# Install dependencies
npm install --production

# Install PM2
npm install -g pm2

# Start application
pm2 start index.js --name alpha-backend

# Set up auto-restart
pm2 startup
pm2 save
```

## 📚 Additional Resources

- [Express.js Documentation](https://expressjs.com/)
- [MySQL2 Documentation](https://github.com/sidorares/node-mysql2)
- [Stripe API Reference](https://stripe.com/docs/api)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)

## 🆘 Getting Help

If you encounter issues:

1. Check this troubleshooting guide
2. Review error logs
3. Search GitHub issues
4. Ask in project Discord/Slack
5. Create a new GitHub issue

---

**Next Steps**: 
- ✅ Backend setup complete
- 📝 Continue with [Frontend Setup](./FRONTEND_SETUP.md)
- 🗄️ Set up [Database](./DATABASE_SETUP.md)
- 🤖 Configure [ML Service](./ML_SERVICE_SETUP.md)
