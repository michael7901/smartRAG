# Security and Configuration Guide

## ⚠️ IMPORTANT: Configuration Files Security

### Configuration Files Structure

This project uses **two separate configuration files**:

1. **`appsettings.json`** (✅ Committed to Git)
   - Contains **NO sensitive information**
   - Uses placeholder values
   - Safe to commit to repository
   - Serves as template for other developers

2. **`appsettings.Development.json`** (🔒 Git Ignored)
   - Contains **REAL API keys and credentials**
   - Overrides `appsettings.json` in development
   - **NEVER committed to Git** (protected by `.gitignore`)
   - Each developer must maintain their own copy

### How It Works

.NET configuration system automatically:
1. Loads `appsettings.json` (base configuration)
2. Loads `appsettings.Development.json` (overrides base in Development environment)
3. Merges them together, with Development settings taking precedence

### Setup Instructions

1. **First Time Setup:**
   ```bash
   # appsettings.Development.json is already in the project
   # Just update it with your real credentials
   ```

2. **Required Credentials:**

   **For AI Features:**
   - Anthropic API Key: https://console.anthropic.com/
   - VoyageAI Embedding API Key: https://console.voyageai.com/

**For Docker Databases (automatically configured):**
- SQL Server: `sa` / `${SQLSERVER_SA_PASSWORD}` (environment variable required)
- MySQL: `root` / `${MYSQL_ROOT_PASSWORD}` (environment variable required)
- PostgreSQL: `postgres` / `${POSTGRES_PASSWORD}` (environment variable required)

3. **Update appsettings.Development.json:**
   ```json
   {
     "AI": {
       "Anthropic": {
         "ApiKey": "sk-ant-YOUR-REAL-KEY-HERE",
         "EmbeddingApiKey": "pa-YOUR-REAL-VOYAGE-KEY-HERE"
       }
     }
   }
   ```

### What's Protected?

The `.gitignore` file protects:
- ✅ `appsettings.Development.json`
- ✅ `appsettings.Production.json`
- ✅ `appsettings.Local.json`
- ✅ All `.secrets.json` files
- ✅ `.env` files

### What's Safe to Commit?

- ✅ `appsettings.json` - Uses only placeholder values
- ✅ `docker-compose.yml` - Test environment credentials (not production)
- ✅ Test database creators - No credentials in code

## 🔐 Security Best Practices

### DO:
- ✅ Keep real API keys in `appsettings.Development.json`
- ✅ Use placeholder values in `appsettings.json`
- ✅ Add new sensitive config files to `.gitignore`
- ✅ Use environment variables in production
- ✅ Rotate API keys regularly

### DON'T:
- ❌ Commit `appsettings.Development.json`
- ❌ Put real API keys in `appsettings.json`
- ❌ Share your API keys in chat/email
- ❌ Use development credentials in production
- ❌ Commit `.env` files

## 🚨 If You Accidentally Committed Secrets

If you accidentally committed a file with secrets:

1. **Immediately revoke the exposed credentials:**
   - Anthropic: https://console.anthropic.com/ → API Keys → Revoke
   - VoyageAI: https://console.voyageai.com/ → Delete key

2. **Remove from Git history:**
   ```bash
   # Remove file from Git but keep local copy
   git rm --cached appsettings.Development.json
   
   # Commit the removal
   git commit -m "Remove sensitive configuration file"
   
   # Push changes
   git push
   ```

3. **Generate new credentials** and update your local `appsettings.Development.json`

## 📋 Credential Checklist

Before committing, verify:
- [ ] No API keys in committed files
- [ ] No database passwords in committed files
- [ ] `appsettings.json` only has placeholder values
- [ ] `appsettings.Development.json` is in `.gitignore`
- [ ] No `.env` files committed

## 🐳 Docker Credentials

Docker credentials are **intentionally simple** because:
- They're for **local testing only**
- Containers run on `localhost`
- Data is isolated in Docker volumes
- **Never use these credentials in production!**

### Environment Variables for Docker

For better security, use environment variables:

**Option 1: Using .env file (Recommended)**
```bash
# Copy the example file
cp env.example .env

# Edit .env with your secure passwords
nano .env

# Start Docker (automatically reads .env)
docker-compose up -d
```

**Option 2: Using export commands**
```bash
# Set secure passwords before starting Docker
export SQLSERVER_SA_PASSWORD="YourSecurePassword123!"
export MYSQL_ROOT_PASSWORD="YourMySQLPassword456!"
export POSTGRES_PASSWORD="YourPostgresPassword789!"

# Start Docker with custom passwords
docker-compose up -d
```

This approach:
- ✅ Avoids hardcoded passwords in code
- ✅ Allows different passwords per environment
- ✅ Follows security best practices
- ✅ Prevents accidental credential exposure

## 📚 Additional Resources

- [ASP.NET Core Configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/)
- [User Secrets in .NET](https://docs.microsoft.com/en-us/aspnet/core/security/app-secrets)
- [Environment Variables](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/#environment-variables)

## Contact


