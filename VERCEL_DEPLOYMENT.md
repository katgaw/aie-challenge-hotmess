# Vercel Deployment Guide

## Deployment Steps

### 1. Install Dependencies

First, make sure all dependencies are installed:

```bash
# Install frontend dependencies
cd frontend-aie-challenge-hot-mess
npm install

# Install Python dependencies (for local testing)
cd ..
uv sync
```

### 2. Vercel Project Settings

When deploying to Vercel, configure these settings:

#### Option A: Root Directory Approach (Recommended)

1. **Root Directory**: Set to `frontend-aie-challenge-hot-mess`
   - Go to Vercel Dashboard → Your Project → Settings → General
   - Set "Root Directory" to `frontend-aie-challenge-hot-mess`
   - Vercel will automatically detect Next.js

2. **Copy Python API**: Since the root is the frontend directory, you need to make the Python API accessible:
   ```bash
   # Create a symlink or copy the api directory
   cd frontend-aie-challenge-hot-mess
   ln -s ../api api
   ```
   Or manually copy the `api/` folder into `frontend-aie-challenge-hot-mess/`

#### Option B: Project Root Approach

1. **Root Directory**: Keep as project root (`.`)
2. **Build Settings**: Manually configure:
   - Framework Preset: `Other`
   - Build Command: `cd frontend-aie-challenge-hot-mess && npm install && npm run build`
   - Output Directory: `frontend-aie-challenge-hot-mess/.next`
   - Install Command: `cd frontend-aie-challenge-hot-mess && npm install`

### 3. Environment Variables

Set `OPENAI_API_KEY` in Vercel:
- Go to Vercel Dashboard → Your Project → Settings → Environment Variables
- Add `OPENAI_API_KEY` with your OpenAI API key
- Make sure it's available for Production, Preview, and Development

### 4. Deploy

```bash
# From project root
vercel
```

Or push to GitHub and Vercel will auto-deploy.

## How It Works

- The `vercel.json` file routes `/api/*` requests to the Python serverless function
- The FastAPI app uses `mangum` to work as a Vercel serverless function
- All other requests are handled by Next.js
- Python files in `api/` are automatically detected as serverless functions

## Troubleshooting

- **Python API not found**: Make sure the `api/` directory is accessible from the root directory Vercel is using
- **Build errors**: Check that all dependencies in `api/requirements.txt` are installed (Vercel handles this automatically)
- **API routes not working**: Verify that `mangum` is in `api/requirements.txt` and the handler is exported in `api/index.py`

