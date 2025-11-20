# Vercel Frontend Setup Guide (Separate Deployment)

This guide explains how to configure your frontend Vercel deployment to connect to a separately deployed backend.

## Step 1: Get Your Backend URL

1. Go to your **backend Vercel project** dashboard
2. Navigate to **Settings** → **Domains**
3. Copy your backend URL (e.g., `https://your-backend.vercel.app` or your custom domain)
4. Make sure your backend API endpoint is accessible at: `https://your-backend.vercel.app/api/chat`

## Step 2: Configure Frontend Environment Variable

1. Go to your **frontend Vercel project** dashboard
2. Navigate to **Settings** → **Environment Variables**
3. Click **Add New**
4. Add the following environment variable:
   - **Key**: `NEXT_PUBLIC_BACKEND_URL`
   - **Value**: Your backend URL (e.g., `https://your-backend.vercel.app`)
   - **Environment**: Select all (Production, Preview, Development)

⚠️ **Important**: 
- Do NOT include a trailing slash in the URL
- The variable name must start with `NEXT_PUBLIC_` to be accessible in the browser
- Make sure to add it to all environments (Production, Preview, Development)

## Step 3: Redeploy

After adding the environment variable:

1. Go to **Deployments** tab
2. Click the **⋯** (three dots) on your latest deployment
3. Click **Redeploy**
4. Or push a new commit to trigger a new deployment

## Step 4: Verify Connection

1. Open your frontend URL
2. Fill out the questionnaire
3. Try sending a message in the chat
4. Check the browser console (F12) for any errors
5. If you see CORS errors, make sure your backend has CORS enabled (it should already be configured)

## How It Works

- **With `NEXT_PUBLIC_BACKEND_URL` set**: Frontend calls `https://your-backend.vercel.app/api/chat`
- **Without `NEXT_PUBLIC_BACKEND_URL`**: Frontend uses relative path `/api/chat` (for same-project deployments)
- **In development**: Uses `localhost:8000` via Next.js rewrites

## Troubleshooting

### CORS Errors
If you see CORS errors, verify your backend (`api/index.py`) has:
```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # Or specify your frontend domain
    allow_methods=["*"],
    allow_headers=["*"],
)
```

### 404 Errors
- Verify your backend URL is correct (no trailing slash)
- Check that your backend is deployed and accessible
- Test the backend endpoint directly: `https://your-backend.vercel.app/api/health`

### Environment Variable Not Working
- Make sure the variable name starts with `NEXT_PUBLIC_`
- Redeploy after adding the variable
- Check that it's enabled for the correct environment (Production/Preview/Development)

