# KompanionAI OAuth Backend

A minimal serverless function for secure Google Calendar OAuth token exchange.

## Why This Exists

This tiny backend keeps your Google OAuth Client Secret secure. Your local-first PWA calls this endpoint to exchange authorization codes for access tokens without exposing the secret in the browser.

## Features

- ✅ Single endpoint: `/api/calendar-token`
- ✅ Stateless (no database required)
- ✅ ~120 lines of code
- ✅ Free tier compatible
- ✅ Handles token exchange and refresh

## Current Deployment

**Your backend is deployed on Render at:**
```
https://serverless-bkend.onrender.com
```

## Getting Google OAuth Credentials

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project (or select existing)
3. Enable **Google Calendar API**:
   - APIs & Services → Library → Search "Google Calendar API" → Enable
4. Create OAuth Credentials:
   - APIs & Services → Credentials → Create Credentials → OAuth 2.0 Client ID
   - Application type: **Web application**
   - Authorized redirect URIs: Add your PWA URL (e.g., `http://localhost:5173/calendar-callback`)
5. Copy your **Client ID** and **Client Secret**

## API Endpoints

### POST /api/calendar-token

**Exchange authorization code for tokens:**
```json
{
  "action": "exchange",
  "code": "authorization-code-from-google",
  "redirectUri": "http://localhost:5173/calendar-callback"
}
```

**Response:**
```json
{
  "access_token": "ya29.xxx",
  "refresh_token": "1//xxx",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```

**Refresh access token:**
```json
{
  "action": "refresh",
  "refreshToken": "1//xxx"
}
```

**Response:**
```json
{
  "access_token": "ya29.xxx",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```

## Render Deployment Configuration

Your backend is already deployed on Render. If you need to redeploy or update:

### Environment Variables on Render

Make sure these are set in your Render dashboard:

- `GOOGLE_CLIENT_ID`: Your Google OAuth Client ID
- `GOOGLE_CLIENT_SECRET`: Your Google OAuth Client Secret (NEVER commit this!)

### Render Service Configuration

- **Build Command**: (leave empty - no build needed)
- **Start Command**: (leave empty - serverless function)
- **Environment**: Node 18+

## Security Notes

- ✅ Client Secret never leaves the server
- ✅ CORS enabled (adjust in production)
- ✅ No data storage (stateless)
- ✅ Only handles token exchange

## Troubleshooting

**"Missing OAuth credentials" error:**
- Check environment variables are set in your Render dashboard

**CORS errors:**
- Update `Access-Control-Allow-Origin` in `api/calendar-token.js` to your PWA domain
- For local development, `*` (allow all) is fine

**Token exchange failed:**
- Verify redirect URI matches exactly what you configured in Google Console
- Check that Google Calendar API is enabled
- Make sure your Render backend is awake (free tier sleeps after inactivity)

## Alternative Deployment Platforms

While this is deployed on Render, you can also deploy to:

- **Vercel**: `vercel` (requires Vercel CLI)
- **Netlify**: Drag and drop the `serverless-backend` folder
- **Railway**: Connect GitHub repo

All platforms need the same environment variables.

## Future Extensions

This backend pattern can be reused for:
- Other Google APIs (Gmail, Drive, etc.)
- Microsoft OAuth
- Any OAuth provider requiring Client Secret
