# 🚀 Deployment Guide - SaaS Website Generator

## Quick Deploy to Railway

Railway is the best Heroku alternative with free tier support.

### Prerequisites
- GitHub account
- Railway account (https://railway.app - free tier available)
- Google Gemini API key

---

## Step 1: Push Code to GitHub

```bash
cd your-project-folder
git init
git add .
git commit -m "Initial commit - SaaS website generator"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/saas-gen-app.git
git push -u origin main
```

---

## Step 2: Deploy on Railway

### Option A: Using Railway Dashboard (Easiest)

1. Go to https://railway.app/dashboard
2. Click **"New Project"** → **"Deploy from GitHub repo"**
3. Select your repository
4. Railway will auto-detect Node.js and start building
5. Wait for deployment to complete

### Option B: Using Railway CLI

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login
railway login

# Link project
railway init

# Deploy
railway up
```

---

## Step 3: Configure Environment Variables

After deployment starts:

1. Go to your Railway project dashboard
2. Click **"Variables"** tab
3. Add these variables:
   - `GEMINI_API_KEY` → Your Google Gemini API key
   - `JWT_SECRET` → Generate with: `node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"`
   - `NODE_ENV` → `production`
   - `PORT` → `3000` (Railway will override this)

4. Click **"Redeploy"** to apply changes

---

## Step 4: Test Your Live App

Your app will be live at: `https://your-app-xyz.up.railway.app`

### Test endpoints:

```bash
# Landing page
curl https://your-app-xyz.up.railway.app/

# Register user
curl -X POST https://your-app-xyz.up.railway.app/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","password":"testpass123"}'

# Login
curl -X POST https://your-app-xyz.up.railway.app/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"testuser","password":"testpass123"}'

# Get token from login response, then test generation:
curl -X POST https://your-app-xyz.up.railway.app/api/generate \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -d '{"prompt":"Create a portfolio website for a web developer"}'
```

---

## Step 5: Custom Domain (Optional)

1. In Railway dashboard, go to **"Settings"**
2. Under **"Domains"**, add your custom domain
3. Follow DNS setup instructions
4. Domain will work within 24 hours

---

## Troubleshooting

### Build fails?
- Check Railway logs: Click project → **"Logs"** tab
- Ensure `package.json` and `package-lock.json` are committed
- Verify Node.js version compatibility

### App crashes after deploy?
```bash
# View logs
railway logs

# Common issues:
# - Missing GEMINI_API_KEY → Add to variables
# - Wrong JWT_SECRET → Verify it's set
# - Port binding → Check PORT variable
```

### AI generation returns errors?
- Test GEMINI_API_KEY locally: `npm run dev`
- Verify API key has access to Gemini API
- Check quota limits on Google Cloud Console

### Can't login after deploy?
- Clear browser cache/localStorage
- Token might have expired → Login again
- Check JWT_SECRET is consistent across restarts

---

## Production Improvements Needed

⚠️ **Current limitations (in-memory storage, no database)**

For production upgrade:

1. **Add Database**
   - Install: `npm install mongodb` or `npm install pg`
   - Store users, generated websites, usage logs
   - Track payment/usage for $20 billing

2. **Add Payment Processing**
   - Re-enable Stripe/Razorpay integration
   - Track usage per user
   - Implement $20 per website charge

3. **Add Usage Tracking**
   - Log all website generations
   - Count usage per user
   - Bill automatically

4. **Security Hardening**
   - Use bcrypt for password (✓ already done)
   - Use environment variables for secrets (✓ already done)
   - Add rate limiting on API
   - Add CSRF protection
   - Use HTTPS only (✓ Railway handles this)

---

## Next Steps

After deployment is live:

1. Test all features thoroughly
2. Monitor Railway dashboard for errors
3. Plan database migration
4. Re-enable payment system
5. Add analytics tracking

---

## Support

- Railway Docs: https://docs.railway.app
- Emergency: Check Railway status page or contact support

Your SaaS is now live! 🎉
