# 🔧 Backend-Frontend Integration Configuration Guide

## ✅ Configuration Complete!

The backend and AI engine have been properly configured to work with the frontend. Here's what was done:

---

## 📋 Changes Made

### 1. **Frontend Environment Configuration** (`.env.local`)
Created environment configuration file with:
- `NEXT_PUBLIC_API_URL=http://localhost:3000/api` - Backend API endpoint
- `NEXT_PUBLIC_AI_ENGINE_URL=http://localhost:5000` - AI Engine endpoint
- Environment variables for API keys and configuration

### 2. **Frontend API Service** (`src/lib/api-service.ts`)
Updated to use environment variables instead of hardcoded URLs:
```typescript
const API_BASE_URL = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:3000/api';
const AI_ENGINE_URL = process.env.NEXT_PUBLIC_AI_ENGINE_URL || 'http://localhost:5000';
```

### 3. **Next.js Configuration** (`next.config.ts`)
Added:
- API proxy rewrites for CORS handling
- Environment variable configuration
- Image optimization settings

### 4. **Backend CORS Configuration** (`backend/server.js`)
Updated CORS to allow frontend connections:
```javascript
app.use(cors({
  origin: [
    'http://localhost:3001',  // Frontend
    'http://localhost:3000',  // Backend
    'http://127.0.0.1:3001',
    'http://127.0.0.1:3000'
  ],
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'OPTIONS'],
  allowedHeaders: ['Content-Type', 'Authorization']
}));
```

### 5. **AI Engine CORS Configuration** (`ai-engine/app.py`)
Updated Flask CORS to allow frontend and backend connections:
```python
CORS(app, resources={
    r"/*": {
        "origins": [
            "http://localhost:3001",  # Frontend
            "http://localhost:3000",  # Backend
        ],
        "methods": ["GET", "POST", "PUT", "DELETE", "OPTIONS"],
        "allow_headers": ["Content-Type", "Authorization"],
        "supports_credentials": True
    }
})
```

---

## 🚀 How to Start the Application

### Option 1: Automated Start (Recommended)
```powershell
# Start all services at once
.\start-all.ps1
```

### Option 2: Manual Start
Open 3 separate terminals:

**Terminal 1 - Backend (Node.js)**
```powershell
cd backend
npm start
```

**Terminal 2 - AI Engine (Python Flask)**
```powershell
cd ai-engine
python app.py
```

**Terminal 3 - Frontend (Next.js)**
```powershell
cd frontend
npm run dev
```

---

## 🌐 Service URLs

| Service | URL | Port |
|---------|-----|------|
| **Frontend** | http://localhost:3001 | 3001 |
| **Backend API** | http://localhost:3000 | 3000 |
| **AI Engine** | http://localhost:5000 | 5000 |

### Health Check Endpoints
- Backend: http://localhost:3000/health
- AI Engine: http://localhost:5000/health
- Frontend Test Page: http://localhost:3001/test

---

## 🔍 Testing the Integration

### 1. **Health Checks**
```powershell
# Test Backend
curl http://localhost:3000/health

# Test AI Engine
curl http://localhost:5000/health

# Test Frontend
# Open browser: http://localhost:3001
```

### 2. **API Integration Test**
Visit the test page: http://localhost:3001/test

This page will test:
- ✅ Backend connectivity
- ✅ AI Engine connectivity
- ✅ Symptom analysis functionality
- ✅ Authentication flow

### 3. **Manual API Test**
```powershell
# Test symptom analysis
curl -X POST http://localhost:3000/api/voice-assistant/analyze `
  -H "Content-Type: application/json" `
  -d '{
    "userId": "test-user",
    "message": "I have fever and headache",
    "language": "en"
  }'
```

---

## 🛠️ Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                     User Browser                             │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│              Frontend (Next.js - Port 3001)                  │
│  - React Components                                          │
│  - API Service (api-service.ts)                             │
│  - Voice Assistant API (voice-assistant-api.ts)             │
└──────────────┬──────────────────────┬───────────────────────┘
               │                      │
               ▼                      ▼
┌──────────────────────────┐  ┌─────────────────────────────┐
│ Backend (Node.js - 3000) │  │ AI Engine (Flask - 5000)    │
│ - Express API            │  │ - Symptom Analysis          │
│ - Authentication         │  │ - Disease Prediction        │
│ - Database (PostgreSQL)  │  │ - Healthcare Routing        │
│ - Voice Assistant Route  │  │ - ML Models                 │
└──────────────────────────┘  └─────────────────────────────┘
```

---

## 📝 API Endpoints

### Backend API (http://localhost:3000/api)

#### Authentication
- `POST /api/auth/send-otp` - Send OTP to phone
- `POST /api/auth/login` - Login with OTP
- `POST /api/auth/register` - Register new user
- `GET /api/auth/profile` - Get user profile

#### Voice Assistant (Integrated with OpenAI)
- `POST /api/voice-assistant/analyze` - Analyze symptoms with AI
- `GET /api/voice-assistant/conversations/:userId` - Get conversation history
- `GET /api/voice-assistant/conversation/:conversationId` - Get specific conversation
- `POST /api/voice-assistant/feedback` - Submit feedback

#### Healthcare Services
- `POST /api/resources/find` - Find healthcare facilities
- `POST /api/resources/emergency/ambulance` - Request ambulance
- `GET /api/triage/stats` - Get triage statistics
- `GET /api/asha/dashboard` - ASHA worker dashboard
- `GET /api/teleconsult/doctors` - Get available doctors
- `POST /api/teleconsult/book` - Book teleconsultation

### AI Engine API (http://localhost:5000)

#### Core AI Features
- `POST /analyze` - Analyze symptoms (legacy endpoint)
- `GET /health` - Health check
- `GET /user-history/:userId` - Get user history
- `GET /healthcare-facilities` - Get healthcare facilities
- `POST /voice-to-text` - Convert voice to text
- `POST /image-analysis` - Analyze medical images
- `POST /translate` - Translate text
- `GET /models/status` - Get model status
- `POST /emergency-alert` - Send emergency alert

---

## 🔐 Environment Variables

### Frontend (`.env.local`)
```env
NEXT_PUBLIC_API_URL=http://localhost:3000/api
NEXT_PUBLIC_AI_ENGINE_URL=http://localhost:5000
NODE_ENV=development
NEXT_PUBLIC_GEMINI_API_KEY=Enter your api key
```

### Backend (`.env`)
```env
PORT=3000
NODE_ENV=development
DB_HOST=localhost
DB_PORT=5432
DB_NAME=healthbridge
DB_USER=postgres
DB_PASSWORD=kali
JWT_SECRET=your jet uri
JWT_EXPIRES_IN=7d
OPENAI_API_KEY=sk-proj-...
OPENAI_MODEL=gpt-4o-mini
GEMINI_API_KEY=Enter your api key
```

### AI Engine (No .env needed currently)
The AI Engine runs standalone with default configuration.

---

## 🐛 Troubleshooting

### Issue: "Failed to fetch" or CORS errors
**Solution:** 
1. Ensure all services are running
2. Check CORS configuration in backend and AI engine
3. Verify URLs in `.env.local` match running services

### Issue: "Connection refused"
**Solution:**
1. Check if backend is running on port 3000
2. Check if AI engine is running on port 5000
3. Check if frontend is running on port 3001
4. Verify no port conflicts

### Issue: "Module not found" errors
**Solution:**
```powershell
# Reinstall frontend dependencies
cd frontend
rm -rf node_modules .next
npm install

# Reinstall backend dependencies
cd ../backend
rm -rf node_modules
npm install

# Reinstall AI engine dependencies
cd ../ai-engine
pip install -r requirements.txt
```

### Issue: Environment variables not loading
**Solution:**
1. Ensure `.env.local` exists in `frontend/` directory
2. Restart the frontend development server
3. Clear Next.js cache: `rm -rf .next`

---

## ✅ Verification Checklist

- [ ] Backend running on port 3000
- [ ] AI Engine running on port 5000
- [ ] Frontend running on port 3001
- [ ] `.env.local` file exists in frontend directory
- [ ] CORS configured in backend
- [ ] CORS configured in AI engine
- [ ] Health checks passing for all services
- [ ] Test page accessible at http://localhost:3001/test
- [ ] API calls working from frontend to backend
- [ ] Symptom analysis working end-to-end

---

## 🎯 Next Steps

1. **Start all services** using `.\start-all.ps1`
2. **Visit** http://localhost:3001 to see the application
3. **Test** the symptom analysis feature
4. **Check** the test page at http://localhost:3001/test
5. **Monitor** console logs for any errors

---

## 📞 Support

If you encounter any issues:
1. Check the console logs in all three terminals
2. Verify all environment variables are set correctly
3. Ensure all dependencies are installed
4. Check that no other services are using ports 3000, 3001, or 5000

---

**🎉 Your HealthBridge AI platform is now properly configured!**

The frontend, backend, and AI engine are now integrated and ready to use.
