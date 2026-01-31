# 🎨 Frontend Setup Guide

Complete guide to set up the Alpha frontend application.

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Environment Configuration](#environment-configuration)
- [Running the Application](#running-the-application)
- [Building for Production](#building-for-production)
- [Project Structure](#project-structure)
- [Styling Guide](#styling-guide)
- [Component Development](#component-development)
- [Troubleshooting](#troubleshooting)

## 🎯 Prerequisites

Before setting up the frontend, ensure you have:

- **Node.js** (v18.0.0 or higher)
- **npm** (v9.0.0 or higher) or **yarn**
- **Git** - For cloning the repository
- **Backend Server** - Must be running on port 3000
- **Modern Browser** - Chrome, Firefox, Safari, or Edge (latest versions)

### Check Prerequisites

```bash
# Check Node.js version
node --version
# Should output: v18.0.0 or higher

# Check npm version
npm --version
# Should output: v9.0.0 or higher

# Check if backend is running
curl http://localhost:3000/health
# Should return: {"status": "ok"}
```

## 📦 Installation

### Step 1: Navigate to Frontend Directory

```bash
cd Alpha/front-end
```

### Step 2: Install Dependencies

```bash
npm install
```

This will install all required packages:

**Core Dependencies:**
- **react** (v19.2.0) - UI library
- **react-dom** (v19.2.0) - React DOM renderer
- **react-router-dom** (v7.13.0) - Routing
- **vite** (v7.2.4) - Build tool & dev server

**UI & Styling:**
- **tailwindcss** (v4.1.18) - CSS framework
- **@tailwindcss/vite** (v4.1.18) - Vite plugin
- **react-icons** (v5.5.0) - Icon library
- **react-hot-toast** (v2.6.0) - Toast notifications

**Features:**
- **@stripe/stripe-js** (v8.7.0) - Payment processing
- **leaflet** (v1.9.4) - Interactive maps
- **leaflet-routing-machine** (v3.2.12) - Map routing
- **react-calendar-timeline** (v0.30.0-beta.4) - Timeline component
- **moment** (v2.30.1) - Date/time manipulation

**Development:**
- **@vitejs/plugin-react** (v5.1.1) - React plugin for Vite
- **eslint** (v9.39.1) - Linting
- **eslint-plugin-react** - React-specific linting rules

### Step 3: Verify Installation

```bash
# Check if node_modules was created
ls node_modules

# Verify package.json
cat package.json
```

## 🔐 Environment Configuration

### Step 1: Create Environment File

```bash
# Create .env file
touch .env

# Or on Windows PowerShell
New-Item .env -ItemType File
```

### Step 2: Configure Environment Variables

Open `.env` file and add:

```env
# ============================================
# API CONFIGURATION
# ============================================
VITE_API_URL=http://localhost:3000
VITE_API_TIMEOUT=30000

# ============================================
# STRIPE CONFIGURATION
# ============================================
# Get this from: https://dashboard.stripe.com/apikeys
VITE_STRIPE_PUBLIC_KEY=pk_test_51xxxxxxxxxxxxxxxxxxxxx

# ============================================
# MAP CONFIGURATION
# ============================================
VITE_MAP_API_KEY=your_map_api_key_here
VITE_MAP_DEFAULT_LAT=23.3441
VITE_MAP_DEFAULT_LNG=85.3096
VITE_MAP_DEFAULT_ZOOM=13

# ============================================
# CHATBOT CONFIGURATION
# ============================================
VITE_CHATBOT_ENABLED=true
VITE_CHATBOT_POSITION=bottom-right

# ============================================
# FEATURE FLAGS
# ============================================
VITE_ENABLE_ANALYTICS=true
VITE_ENABLE_NOTIFICATIONS=true
VITE_ENABLE_ML_VALIDATION=true

# ============================================
# DEVELOPMENT
# ============================================
VITE_DEV_PORT=5173
VITE_ENABLE_DEBUG=true

# ============================================
# PRODUCTION
# ============================================
VITE_PRODUCTION_URL=https://alpha-platform.com
```

### Step 3: Environment Security

⚠️ **IMPORTANT**: 

- Never commit `.env` file
- Use `.env.example` for template
- All Vite env vars must start with `VITE_`

```bash
# Verify .env is in .gitignore
cat .gitignore | grep .env

# Create .env.example template
cp .env .env.example
# Remove sensitive values from .env.example
```

## 🚀 Running the Application

### Development Mode

```bash
npm run dev
```

The application will start on `http://localhost:5173`

**Expected Output:**
```
  VITE v7.2.4  ready in 324 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

### Access the Application

Open your browser and navigate to:
- **Home**: http://localhost:5173/
- **Manage Space**: http://localhost:5173/manage-space
- **Rent Space**: http://localhost:5173/rent-space
- **Admin**: http://localhost:5173/admin

### Development Features

**Hot Module Replacement (HMR)**
- Changes auto-reload in browser
- Component state preserved
- Fast refresh for React components

**Enable Host Exposure**
```bash
# Access from other devices on network
npm run dev -- --host

# Output will show:
# ➜  Local:   http://localhost:5173/
# ➜  Network: http://192.168.1.x:5173/
```

**Custom Port**
```bash
# Run on different port
npm run dev -- --port 3001
```

## 🏗️ Building for Production

### Step 1: Build the Application

```bash
npm run build
```

This creates an optimized production build in the `dist/` directory.

**Build Output:**
```
vite v7.2.4 building for production...
✓ 1247 modules transformed.
dist/index.html                   0.45 kB │ gzip:  0.29 kB
dist/assets/index-abc123.css     45.23 kB │ gzip: 12.45 kB
dist/assets/index-def456.js     234.56 kB │ gzip: 78.91 kB
✓ built in 3.45s
```

### Step 2: Preview Production Build

```bash
npm run preview
```

The production build will be served on `http://localhost:4173`

### Step 3: Analyze Bundle Size

```bash
# Install analyzer
npm install --save-dev rollup-plugin-visualizer

# Add to vite.config.js
import { visualizer } from 'rollup-plugin-visualizer';

export default {
  plugins: [
    react(),
    visualizer()
  ]
}

# Build and view stats
npm run build
# Open stats.html in browser
```

## 📁 Project Structure

```
front-end/
├── public/                      # Static assets
│   ├── favicon.ico
│   └── images/
│
├── src/
│   ├── assets/                 # Images, fonts, etc.
│   │   └── logo.svg
│   │
│   ├── components/             # React components
│   │   ├── Navbar.jsx         # Global navigation
│   │   │
│   │   ├── admin/             # Admin dashboard
│   │   │   ├── Dashboard.jsx
│   │   │   ├── Images.jsx
│   │   │   ├── Login.jsx
│   │   │   └── Map.jsx
│   │   │
│   │   ├── chatbot/           # AI Chatbot
│   │   │   └── Chatbot.jsx
│   │   │
│   │   ├── form/              # Reusable form components
│   │   │   └── Button.jsx
│   │   │
│   │   ├── home/              # Landing page
│   │   │   ├── Features.jsx
│   │   │   ├── Footer.jsx
│   │   │   ├── Hero.jsx
│   │   │   └── Solutions.jsx
│   │   │
│   │   ├── manage-space/      # Space owner dashboard
│   │   │   ├── Dashboard.jsx
│   │   │   ├── Login.jsx
│   │   │   ├── Map.jsx
│   │   │   ├── Nav.jsx
│   │   │   └── SignUp.jsx
│   │   │
│   │   └── rent-space/        # Renter dashboard
│   │       ├── Book.jsx
│   │       ├── Booked.jsx
│   │       ├── Dashboard.jsx
│   │       ├── History.jsx
│   │       ├── Login.jsx
│   │       ├── Nav.jsx
│   │       └── SignUp.jsx
│   │
│   ├── pages/                  # Page components
│   │   ├── Admin.jsx
│   │   ├── Home.jsx
│   │   ├── ManageSpace.jsx
│   │   └── RentSpace.jsx
│   │
│   ├── util/                   # Utilities
│   │   ├── fetchData.js       # API calls
│   │   └── GlobalContextComponent.jsx  # Global state
│   │
│   ├── App.jsx                 # Main app component
│   ├── index.jsx               # Entry point
│   └── index.css               # Global styles
│
├── .env                        # Environment variables
├── .gitignore                  # Git ignore rules
├── eslint.config.js            # ESLint configuration
├── index.html                  # HTML template
├── package.json                # Dependencies
├── tailwind.config.js          # Tailwind configuration
├── vite.config.js              # Vite configuration
└── README.md                   # Frontend documentation
```

## 🎨 Styling Guide

### Tailwind CSS

**Using Utility Classes:**

```jsx
// Example component with Tailwind
function Button({ children, onClick }) {
  return (
    <button
      onClick={onClick}
      className="px-6 py-3 bg-blue-600 text-white rounded-lg 
                 hover:bg-blue-700 transition-colors duration-200
                 focus:outline-none focus:ring-2 focus:ring-blue-500"
    >
      {children}
    </button>
  );
}
```

**Custom Styles in index.css:**

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  .btn-primary {
    @apply px-6 py-3 bg-blue-600 text-white rounded-lg hover:bg-blue-700;
  }
  
  .card {
    @apply bg-white shadow-lg rounded-xl p-6;
  }
}
```

### Responsive Design

```jsx
<div className="
  grid grid-cols-1           /* Mobile: 1 column */
  md:grid-cols-2             /* Tablet: 2 columns */
  lg:grid-cols-3             /* Desktop: 3 columns */
  gap-6
">
  {/* Content */}
</div>
```

### Dark Mode Support

```jsx
<div className="bg-white dark:bg-gray-900 text-black dark:text-white">
  {/* Content */}
</div>
```

## 🧩 Component Development

### Creating a New Component

```jsx
// src/components/example/MyComponent.jsx
import { useState } from 'react';

function MyComponent({ title }) {
  const [count, setCount] = useState(0);
  
  return (
    <div className="p-4">
      <h2 className="text-2xl font-bold">{title}</h2>
      <p>Count: {count}</p>
      <button 
        onClick={() => setCount(count + 1)}
        className="btn-primary"
      >
        Increment
      </button>
    </div>
  );
}

export default MyComponent;
```

### Using Context for State Management

```jsx
// src/util/GlobalContextComponent.jsx
import { createContext, useContext, useState } from 'react';

const GlobalContext = createContext();

export function GlobalProvider({ children }) {
  const [user, setUser] = useState(null);
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  
  const value = {
    user,
    setUser,
    isAuthenticated,
    setIsAuthenticated
  };
  
  return (
    <GlobalContext.Provider value={value}>
      {children}
    </GlobalContext.Provider>
  );
}

export function useGlobal() {
  return useContext(GlobalContext);
}
```

### API Integration

```jsx
// src/util/fetchData.js
const API_URL = import.meta.env.VITE_API_URL;

export async function fetchData(endpoint, options = {}) {
  try {
    const response = await fetch(`${API_URL}${endpoint}`, {
      ...options,
      headers: {
        'Content-Type': 'application/json',
        ...options.headers,
      },
    });
    
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    return await response.json();
  } catch (error) {
    console.error('Fetch error:', error);
    throw error;
  }
}

// Usage in component
import { fetchData } from '../util/fetchData';

async function loadSpaces() {
  const spaces = await fetchData('/api/rent-space/get-marker', {
    method: 'POST',
    body: JSON.stringify({ latitude: 23.3441, longitude: 85.3096 })
  });
}
```

## 🐛 Troubleshooting

### Port Already in Use

```bash
# Error: Port 5173 is already in use

# Solution 1: Kill process
# On Windows
netstat -ano | findstr :5173
taskkill /PID <PID> /F

# On Linux/Mac
lsof -ti:5173 | xargs kill -9

# Solution 2: Use different port
npm run dev -- --port 3001
```

### Module Not Found Errors

```bash
# Error: Cannot find module 'react'

# Solution: Reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

### Vite Build Errors

```bash
# Error: Failed to resolve import

# Solution 1: Clear cache
rm -rf node_modules/.vite
npm run dev

# Solution 2: Update vite
npm update vite
```

### API Connection Errors

```bash
# Error: Network request failed

# Check backend is running
curl http://localhost:3000/health

# Verify VITE_API_URL in .env
echo $VITE_API_URL

# Check CORS configuration in backend
```

### Styling Not Applied

```bash
# Error: Tailwind classes not working

# Solution 1: Restart dev server
npm run dev

# Solution 2: Clear Tailwind cache
rm -rf node_modules/.cache

# Solution 3: Verify tailwind.config.js
```

### Environment Variables Not Loading

```bash
# Error: import.meta.env.VITE_API_URL is undefined

# Remember: Vite env vars must start with VITE_
# Correct: VITE_API_URL
# Wrong: API_URL

# Restart dev server after changing .env
```

## 🧪 Testing

### Component Testing

```bash
# Install testing dependencies
npm install --save-dev vitest @testing-library/react @testing-library/jest-dom

# Run tests
npm run test
```

### E2E Testing with Cypress

```bash
# Install Cypress
npm install --save-dev cypress

# Open Cypress
npx cypress open

# Run tests
npx cypress run
```

## 🚀 Deployment

### Deploy to Vercel

```bash
# Install Vercel CLI
npm install -g vercel

# Login
vercel login

# Deploy
vercel

# Production deployment
vercel --prod
```

### Deploy to Netlify

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Login
netlify login

# Build and deploy
netlify deploy --prod --dir=dist
```

### Deploy to GitHub Pages

```bash
# Install gh-pages
npm install --save-dev gh-pages

# Add to package.json scripts:
"deploy": "vite build && gh-pages -d dist"

# Deploy
npm run deploy
```

## 📊 Performance Optimization

### Code Splitting

```jsx
import { lazy, Suspense } from 'react';

const Dashboard = lazy(() => import('./components/Dashboard'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Dashboard />
    </Suspense>
  );
}
```

### Image Optimization

```jsx
// Use appropriate image formats
<img 
  src="/image.webp" 
  alt="Description"
  loading="lazy"
  width={800}
  height={600}
/>
```

### Bundle Size Reduction

```bash
# Analyze bundle
npm run build
npx vite-bundle-visualizer

# Remove unused dependencies
npm install -g depcheck
depcheck

# Tree-shake imports
import { specific } from 'library'; // Good
import * as all from 'library';    // Bad
```

## 📚 Additional Resources

- [React Documentation](https://react.dev/)
- [Vite Documentation](https://vitejs.dev/)
- [Tailwind CSS Documentation](https://tailwindcss.com/)
- [React Router Documentation](https://reactrouter.com/)
- [Stripe.js Documentation](https://stripe.com/docs/js)
- [Leaflet Documentation](https://leafletjs.com/)

## 🆘 Getting Help

If you encounter issues:

1. Check this troubleshooting guide
2. Review browser console for errors
3. Check network tab for API issues
4. Search GitHub issues
5. Ask in project Discord/Slack
6. Create a new GitHub issue

---

**Next Steps:**
- ✅ Frontend setup complete
- 🎨 Start developing components
- 🧪 Write tests for your features
- 🚀 Deploy to production
