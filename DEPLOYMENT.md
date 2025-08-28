# YourComfortStay - Deployment Fix Guide

## Issues Fixed

### 1. Server Port Configuration

- **Problem**: Server was hardcoded to port 7000
- **Fix**: Now uses `process.env.PORT` for dynamic port assignment by Render
- **Location**: `backend/src/index.ts`

### 2. Health Check Endpoint

- **Problem**: Missing health check endpoint for Render monitoring
- **Fix**: Added `/healthz` endpoint that returns server status
- **Location**: `backend/src/index.ts`

### 3. Build Scripts for Single Service Deployment

- **Problem**: Backend build needed npm install for Render's environment
- **Fix**: Updated backend build script to include dependency installation
- **Location**: `backend/package.json`

### 4. Environment Variables

- **Problem**: No documentation of required environment variables
- **Fix**: Created `.env.example` file with all required variables
- **Location**: `.env.example`

## Render Configuration

### Environment Variables Required

Set these in your Render dashboard under Environment Variables:

```
MONGODB_CONNECTION_STRING=mongodb+srv://username:password@cluster.mongodb.net/yourdatabase
FRONTEND_URL=https://yourcomfortstay.onrender.com
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
JWT_SECRET_KEY=your_jwt_secret_key_here
STRIPE_API_KEY=your_stripe_api_key_here
```

### Build & Deploy Settings

1. **Build Command**: `cd frontend && npm install && npm run build && cd ../backend && npm install && npm run build`
2. **Start Command**: `cd backend && npm start`
3. **Health Check Path**: `/healthz`

## Common Deployment Issues & Solutions

### Issue 1: "Cannot find module" errors

**Solution**: Ensure all dependencies are listed in package.json and npm install runs before build

### Issue 2: Static files not serving

**Solution**: Verify frontend build is successful and static file path is correct

### Issue 3: Database connection fails

**Solution**: Check MONGODB_CONNECTION_STRING environment variable

### Issue 4: CORS errors

**Solution**: Ensure FRONTEND_URL environment variable matches your Render URL

## Testing Deployment

1. Check health endpoint: `curl https://yourcomfortstay.onrender.com/healthz`
2. Verify API endpoints work: `curl https://yourcomfortstay.onrender.com/api/auth/validate-token`
3. Test frontend loads: Visit your Render URL in browser

## Local Development

1. Copy `.env.example` to `.env` and fill in your values
2. Install dependencies: `npm install` in both frontend and backend directories
3. Run development servers:
   - Backend: `cd backend && npm run dev`
   - Frontend: `cd frontend && npm run dev`

## Troubleshooting

If deployment still fails:

1. Check Render logs for specific error messages
2. Verify all environment variables are set correctly
3. Ensure MongoDB database is accessible from Render
4. Check that Cloudinary and Stripe credentials are valid
