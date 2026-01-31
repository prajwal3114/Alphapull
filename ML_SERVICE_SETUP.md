# 🤖 ML Service Setup Guide

Complete guide to set up the Alpha ML (Machine Learning) service for image validation.

## 📋 Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Service](#running-the-service)
- [API Endpoints](#api-endpoints)
- [Model Information](#model-information)
- [Troubleshooting](#troubleshooting)
- [Development](#development)

## 🎯 Overview

The ML Service is a Python-based FastAPI application that provides:

- **Image Validation**: Verify uploaded space images meet quality standards
- **Content Detection**: Identify inappropriate or invalid content
- **Quality Analysis**: Check image dimensions, clarity, and composition
- **Real-time Processing**: Fast API responses for validation requests

### Technology Stack

- **FastAPI** (v0.104.1) - Modern web framework
- **Uvicorn** (v0.24.0) - ASGI server
- **OpenCV** (v4.8.1.78) - Computer vision
- **NumPy** (v1.24.3) - Numerical computing
- **Scikit-learn** (v1.3.2) - Machine learning algorithms
- **Matplotlib** (v3.8.2) - Visualization (optional)

## 🎯 Prerequisites

Before setting up the ML service, ensure you have:

- **Python** (v3.9 or higher, v3.11 recommended)
- **pip** (Python package installer)
- **Virtual environment** (recommended)
- **Git** - For cloning the repository
- **Backend server** - Running on port 3000

### Check Prerequisites

```bash
# Check Python version
python --version
# Should output: Python 3.9.x or higher

# Check pip version
pip --version
# Should output: pip 21.x or higher

# Alternative python command
python3 --version
```

### Install Python (if needed)

**Windows:**
```bash
# Download from python.org
https://www.python.org/downloads/

# Or use Chocolatey
choco install python

# Verify installation
python --version
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv
python3 --version
```

**macOS:**
```bash
# Install using Homebrew
brew install python@3.11

# Verify installation
python3 --version
```

## 📦 Installation

### Step 1: Navigate to ML Service Directory

```bash
cd Alpha/back-end/ml-service
```

### Step 2: Create Virtual Environment

**Windows:**
```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
.\venv\Scripts\activate

# You should see (venv) in your prompt
```

**Linux/macOS:**
```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate

# You should see (venv) in your prompt
```

### Step 3: Upgrade pip

```bash
# Upgrade pip to latest version
python -m pip install --upgrade pip
```

### Step 4: Install Dependencies

```bash
# Install all required packages
pip install -r requirements.txt
```

**requirements.txt contents:**
```txt
fastapi==0.104.1
uvicorn==0.24.0
opencv-python==4.8.1.78
numpy==1.24.3
python-multipart==0.0.6
scikit-learn==1.3.2
matplotlib==3.8.2
```

### Step 5: Verify Installation

```bash
# Check installed packages
pip list

# Verify key packages
pip show fastapi
pip show opencv-python
pip show scikit-learn
```

### Alternative: Install with Conda

```bash
# Create conda environment
conda create -n alpha-ml python=3.11

# Activate environment
conda activate alpha-ml

# Install packages
pip install -r requirements.txt
```

## 🔧 Configuration

### Environment Variables (Optional)

Create `.env` file in `ml-service` directory:

```env
# ============================================
# ML SERVICE CONFIGURATION
# ============================================
HOST=0.0.0.0
PORT=8000
DEBUG=True

# ============================================
# MODEL CONFIGURATION
# ============================================
MODEL_PATH=./model.p
MASK_PATH=./mask_1920_1080.png

# ============================================
# IMAGE VALIDATION SETTINGS
# ============================================
MIN_IMAGE_WIDTH=800
MIN_IMAGE_HEIGHT=600
MAX_IMAGE_SIZE=10485760
# 10MB = 10 * 1024 * 1024 bytes

ALLOWED_FORMATS=jpg,jpeg,png,webp

# ============================================
# QUALITY THRESHOLDS
# ============================================
MIN_QUALITY_SCORE=0.6
MIN_BRIGHTNESS=50
MAX_BRIGHTNESS=250

# ============================================
# CORS SETTINGS
# ============================================
CORS_ORIGINS=http://localhost:3000,http://localhost:5173

# ============================================
# LOGGING
# ============================================
LOG_LEVEL=INFO
LOG_FILE=./logs/ml-service.log
```

### Model Files

Ensure these files exist in the `ml-service` directory:

```
ml-service/
├── model.p                  # Pre-trained ML model
├── mask_1920_1080.png      # Reference mask for validation
├── app.py                  # Main application
├── utils.py                # Utility functions
└── requirements.txt        # Dependencies
```

## 🚀 Running the Service

### Development Mode

```bash
# Make sure virtual environment is activated
# You should see (venv) in prompt

# Run the service
python app.py
```

**Expected Output:**
```
INFO:     Started server process [12345]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
```

### Production Mode

```bash
# Run with Uvicorn directly
uvicorn app:app --host 0.0.0.0 --port 8000 --workers 4

# Or with gunicorn (Linux/macOS)
gunicorn app:app -w 4 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:8000
```

### Run as Background Service

**Windows (using pythonw):**
```bash
start /B pythonw app.py
```

**Linux (using nohup):**
```bash
nohup python app.py > ml-service.log 2>&1 &
```

**Using screen (Linux/macOS):**
```bash
# Start new screen session
screen -S ml-service

# Run application
python app.py

# Detach: Press Ctrl+A, then D
# Reattach: screen -r ml-service
```

### Access the Service

- **API Base URL**: http://localhost:8000
- **API Documentation**: http://localhost:8000/docs
- **Alternative Docs**: http://localhost:8000/redoc

## 🔌 API Endpoints

### Health Check

```http
GET http://localhost:8000/health
```

**Response:**
```json
{
    "status": "healthy",
    "service": "ML Validation Service",
    "version": "1.0.0"
}
```

### Validate Image

```http
POST http://localhost:8000/validate-image
Content-Type: multipart/form-data
```

**Request:**
```bash
curl -X POST http://localhost:8000/validate-image \
  -F "file=@/path/to/image.jpg"
```

**Response (Valid Image):**
```json
{
    "valid": true,
    "quality_score": 0.85,
    "dimensions": {
        "width": 1920,
        "height": 1080
    },
    "file_size": 245680,
    "format": "JPEG",
    "message": "Image validated successfully"
}
```

**Response (Invalid Image):**
```json
{
    "valid": false,
    "quality_score": 0.45,
    "errors": [
        "Image resolution too low",
        "Poor image quality detected"
    ],
    "message": "Image validation failed"
}
```

### Batch Validation

```http
POST http://localhost:8000/validate-batch
Content-Type: multipart/form-data
```

**Request:**
```bash
curl -X POST http://localhost:8000/validate-batch \
  -F "files=@image1.jpg" \
  -F "files=@image2.jpg" \
  -F "files=@image3.jpg"
```

### Get Model Info

```http
GET http://localhost:8000/model-info
```

**Response:**
```json
{
    "model_type": "Image Validation",
    "version": "1.0.0",
    "loaded": true,
    "features": [
        "Quality Assessment",
        "Dimension Validation",
        "Content Detection"
    ]
}
```

## 🧠 Model Information

### Pre-trained Model

The service uses a custom-trained model (`model.p`) for image validation:

**Model Features:**
- Image quality assessment
- Content appropriateness detection
- Dimension and aspect ratio validation
- Blur and noise detection

**Model Training:**
```python
# Example training script (for reference)
import pickle
from sklearn.ensemble import RandomForestClassifier

# Load training data
X_train, y_train = load_training_data()

# Train model
model = RandomForestClassifier(n_estimators=100)
model.fit(X_train, y_train)

# Save model
with open('model.p', 'wb') as f:
    pickle.dump(model, f)
```

### Update Model

To update the model:

1. Train new model using your dataset
2. Save as `model.p`
3. Replace existing model file
4. Restart ML service

## 🧪 Testing

### Test with cURL

```bash
# Test health endpoint
curl http://localhost:8000/health

# Test image validation
curl -X POST http://localhost:8000/validate-image \
  -F "file=@test-image.jpg"
```

### Test with Python

```python
import requests

# Upload image for validation
url = "http://localhost:8000/validate-image"
files = {"file": open("test-image.jpg", "rb")}

response = requests.post(url, files=files)
print(response.json())
```

### Test with Postman

1. Create new POST request
2. URL: `http://localhost:8000/validate-image`
3. Body: form-data
4. Key: `file`, Type: File
5. Select image file
6. Send request

### Unit Tests

```bash
# Install pytest
pip install pytest pytest-asyncio

# Run tests
pytest tests/

# Run with coverage
pytest --cov=app tests/
```

## 🐛 Troubleshooting

### Import Errors

```bash
# Error: No module named 'cv2'

# Solution: Reinstall OpenCV
pip uninstall opencv-python
pip install opencv-python==4.8.1.78

# If still failing, try opencv-python-headless
pip install opencv-python-headless
```

### Port Already in Use

```bash
# Error: Address already in use

# Windows: Find and kill process
netstat -ano | findstr :8000
taskkill /PID <PID> /F

# Linux/Mac
lsof -ti:8000 | xargs kill -9

# Or change port in app.py
uvicorn.run(app, host="0.0.0.0", port=8001)
```

### Model Loading Errors

```bash
# Error: Unable to load model

# Check if model file exists
ls model.p

# Verify file permissions
chmod 644 model.p

# Regenerate model if corrupted
python train_model.py
```

### Memory Errors

```bash
# Error: MemoryError when processing images

# Solution 1: Reduce image size before processing
# Solution 2: Increase system memory
# Solution 3: Process images in batches
# Solution 4: Use image compression
```

### CORS Errors

```python
# Add CORS middleware in app.py
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000", "http://localhost:5173"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

## 🚀 Deployment

### Deploy with Docker

```dockerfile
# Dockerfile
FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libgl1-mesa-glx \
    libglib2.0-0 \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application files
COPY . .

# Expose port
EXPOSE 8000

# Run application
CMD ["python", "app.py"]
```

**Build and Run:**
```bash
# Build image
docker build -t alpha-ml-service .

# Run container
docker run -p 8000:8000 alpha-ml-service
```

### Deploy to Heroku

```bash
# Create Procfile
echo "web: uvicorn app:app --host 0.0.0.0 --port $PORT" > Procfile

# Create runtime.txt
echo "python-3.11.0" > runtime.txt

# Deploy
heroku create alpha-ml-service
git push heroku main
```

### Deploy to AWS Lambda

```bash
# Install Mangum for Lambda
pip install mangum

# Modify app.py
from mangum import Mangum

app = FastAPI()
# ... your routes ...

handler = Mangum(app)
```

## 📊 Performance Optimization

### Async Processing

```python
import asyncio
from concurrent.futures import ThreadPoolExecutor

executor = ThreadPoolExecutor(max_workers=4)

@app.post("/validate-image")
async def validate_image_async(file: UploadFile):
    loop = asyncio.get_event_loop()
    result = await loop.run_in_executor(
        executor,
        process_image,
        file
    )
    return result
```

### Caching

```python
from functools import lru_cache

@lru_cache(maxsize=128)
def load_model():
    with open('model.p', 'rb') as f:
        return pickle.load(f)
```

### Image Optimization

```python
def resize_if_needed(image, max_size=1920):
    height, width = image.shape[:2]
    if width > max_size or height > max_size:
        scale = max_size / max(width, height)
        new_width = int(width * scale)
        new_height = int(height * scale)
        return cv2.resize(image, (new_width, new_height))
    return image
```

## 📚 Additional Resources

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [OpenCV Python Tutorials](https://docs.opencv.org/4.x/d6/d00/tutorial_py_root.html)
- [Scikit-learn Documentation](https://scikit-learn.org/stable/)
- [Uvicorn Documentation](https://www.uvicorn.org/)

## 🆘 Getting Help

If you encounter issues:

1. Check this troubleshooting guide
2. Review application logs
3. Test with sample images
4. Check Python version compatibility
5. Search GitHub issues
6. Create a new issue with error details

---

**Next Steps:**
- ✅ ML Service setup complete
- 🔙 Integrate with [Backend](./BACKEND_SETUP.md)
- 🧪 Test image validation
- 🚀 Deploy to production
