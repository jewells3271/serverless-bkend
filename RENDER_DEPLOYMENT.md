# Deploying to Render

Your OAuth backend is currently deployed at: **https://serverless-bkend.onrender.com**

## If You Need to Redeploy

### Option 1: Via Render Dashboard (Easiest)

1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Find your service: `serverless-bkend`
3. Click **Manual Deploy** → **Deploy latest commit**

### Option 2: New Deployment from Scratch

1. **Create New Web Service**
   - Go to [Render Dashboard](https://dashboard.render.com/)
   - Click **New** → **Web Service**

2. **Connect Repository**
   - Connect your GitHub repo, OR
   - Upload the `serverless-backend` folder directly

3. **Configure Service**
   ```
   Name: serverless-bkend (or your choice)
   Environment: Node
   Build Command: (leave empty)
   Start Command: (leave empty)
   ```

4. **Add Environment Variables**
   - Click **Environment** tab
   - Add these variables:
     ```
     GOOGLE_CLIENT_ID=your-client-id.apps.googleusercontent.com
     GOOGLE_CLIENT_SECRET=your-client-secret-here
     ```

5. **Deploy**
   - Click **Create Web Service**
   - Wait for deployment (usually 2-3 minutes)
   - Copy your URL: `https://your-service.onrender.com`

## Update Your PWA Configuration

After deploying, update your PWA's `.env` file:

```env
VITE_OAUTH_BACKEND_URL=https://your-service.onrender.com/api/calendar-token
```

## Important Notes

- **Free Tier**: Render's free tier spins down after 15 minutes of inactivity
- **First Request**: May take 30-60 seconds to wake up (cold start)
- **No Build Needed**: This is a serverless function, no build step required
- **CORS**: Already configured to accept requests from any origin (`*`)

## Testing Your Deployment

Test the health endpoint:
```bash
curl https://serverless-bkend.onrender.com/health
```

Should return:
```json
{"status":"ok"}
```

## Troubleshooting

**Service won't start:**
- Check that environment variables are set correctly
- Make sure you're using Node 18+

**Token exchange fails:**
- Verify Google OAuth credentials are correct
- Check that redirect URI in Google Console matches your PWA URL
- Ensure Google Calendar API is enabled in Google Cloud Console

**CORS errors:**
- The function already allows all origins (`*`)
- If you need to restrict, edit `api/calendar-token.js` line 14
