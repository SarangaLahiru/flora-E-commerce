# OAuth Configuration Setup

## ⚠️ Important: Secrets Management

This project uses Google OAuth for authentication. **Never commit OAuth credentials to version control!**

## Local Development Setup

### Step 1: Create Local Configuration File

Copy the example file to create your local configuration:

```bash
# From the backend directory
cd backend/src/main/resources
cp application-local.properties.example application-local.properties
```

### Step 2: Add Your OAuth Credentials

Edit `application-local.properties` and add your actual Google OAuth credentials:

```properties
spring.security.oauth2.client.registration.google.client-id=YOUR_ACTUAL_CLIENT_ID
spring.security.oauth2.client.registration.google.client-secret=YOUR_ACTUAL_CLIENT_SECRET
```

### Step 3: Get Google OAuth Credentials

If you don't have OAuth credentials yet:

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project or select an existing one
3. Enable the Google+ API
4. Go to **Credentials** → **Create Credentials** → **OAuth 2.0 Client ID**
5. Configure the OAuth consent screen
6. Add authorized redirect URI: `http://localhost:8080/api/auth/google/callback`
7. Copy the Client ID and Client Secret to your `application-local.properties`

## Production Deployment

For production, use environment variables instead of property files:

```bash
export GOOGLE_CLIENT_ID=your_production_client_id
export GOOGLE_CLIENT_SECRET=your_production_client_secret
```

The application will automatically use these environment variables if they are set.

## How It Works

- `application.properties` contains default configuration with placeholders
- `application-local.properties` (gitignored) overrides with your actual credentials
- Environment variables take precedence over both property files
- The `.gitignore` file prevents `application-local.properties` from being committed

## Security Notes

✅ **Safe to commit:**
- `application.properties` (contains only placeholders)
- `application-local.properties.example` (template file)

❌ **Never commit:**
- `application-local.properties` (contains real credentials)
- Any file with actual OAuth secrets
