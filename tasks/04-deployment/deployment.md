# 🚀 Deployment Guide

Deploy your movie explorer app to the internet so anyone can use it!

---

## Table of Contents

- [What is Deployment?](#what-is-deployment)
- [Deployment Options](#deployment-options)
- [Deploy to Vercel (Recommended)](#deploy-to-vercel-recommended)
- [Deploy to Netlify](#deploy-to-netlify)
- [Custom Domain](#custom-domain)
- [Troubleshooting](#troubleshooting)

---

## What is Deployment?

### Local vs Deployed

**Local Development:**
- Runs on your computer
- Only you can access it
- URL: `http://localhost:3000`
- Stops when you close terminal

**Deployed:**
- Runs on internet server
- Anyone can access it
- URL: `https://your-app.vercel.app`
- Always running

### Why Deploy?

- 🌍 Share with others
- 🎯 Show on portfolio
- 📱 Works on any device
- ⚡ Always available

---

## Deployment Options

### Best Options for Beginners

| Platform | Cost | Setup Time | Best For |
|----------|------|-----------|----------|
| **Vercel** | Free | 5 min | Next.js apps |
| **Netlify** | Free | 10 min | Static sites |
| **GitHub Pages** | Free | 15 min | Static only |
| **Heroku** | Paid | 20 min | Any app |

**Recommended:** Vercel (it's made by the Next.js creators!)

---

## Deploy to Vercel (Recommended)

### Why Vercel?

- ✅ Made for Next.js apps
- ✅ Automatic deployments from GitHub
- ✅ Free tier is generous
- ✅ Super easy setup
- ✅ Real-time logs

### Step-by-Step

#### Step 1: Push to GitHub

First, make sure your code is on GitHub:

```bash
# In your project folder

# Add all files
git add .

# Commit changes
git commit -m "Complete: Movie Explorer app"

# Push to GitHub
git push
```

#### Step 2: Create Vercel Account

1. Go to https://vercel.com
2. Click "Sign Up"
3. Choose "Continue with GitHub"
4. Authorize Vercel to access GitHub
5. Done!

#### Step 3: Import Project

1. You'll see "Import Project" button
2. Click it
3. Enter your GitHub repo URL:
   ```
   https://github.com/YOUR_USERNAME/movie-explorer-learning
   ```
4. Click "Import"

#### Step 4: Configure Environment Variables

1. You'll see "Environment Variables" section
2. Add your TMDB API key:
   ```
   Name: NEXT_PUBLIC_TMDB_API_KEY
   Value: your_api_key_here
   ```

3. Add TMDB base URL:
   ```
   Name: NEXT_PUBLIC_TMDB_BASE_URL
   Value: https://api.themoviedb.org/3
   ```

4. Click "Add"

#### Step 5: Deploy

1. Click "Deploy"
2. Wait for deployment (usually 1-2 minutes)
3. You'll see: "Congratulations! Your app is deployed"
4. Click the URL to see your live app!

### Automatic Deployments

Now whenever you push to GitHub:

```bash
# Make changes locally
# ...

# Push to GitHub
git add .
git commit -m "Add feature"
git push

# Vercel automatically deploys!
```

You'll see deployment progress at vercel.com/dashboard

---

## Deploy to Netlify

### Alternative to Vercel

If you want to try Netlify instead:

#### Step 1: Prepare Project

Netlify works better with a build step:

```bash
# Build the project
bun run build

# This creates a .next folder
```

#### Step 2: Connect GitHub

1. Go to https://netlify.com
2. Click "Sign Up"
3. Choose "Continue with GitHub"
4. Authorize Netlify

#### Step 3: Create New Site

1. Click "New site from Git"
2. Choose your repo
3. Set Build Command: `bun run build`
4. Set Publish Directory: `.next`

#### Step 4: Add Environment Variables

1. Go to Site Settings → Build & Deploy → Environment
2. Add your TMDB keys:
   ```
   NEXT_PUBLIC_TMDB_API_KEY=your_key
   NEXT_PUBLIC_TMDB_BASE_URL=https://api.themoviedb.org/3
   ```

#### Step 5: Deploy

1. Click "Deploy"
2. Wait for build
3. Get your live URL

---

## Custom Domain

### Add Your Own Domain

Once deployed, you can add your own domain name:

```
Vercel URL:    https://movie-explorer-abc123.vercel.app
Custom Domain: https://my-movie-app.com
```

### How to Get a Domain

1. Go to domain registrar:
   - Namecheap (cheap)
   - GoDaddy (popular)
   - Google Domains (simple)

2. Search for domain you want

3. Buy it (usually $10-15/year)

### Connect Domain to Vercel

1. In Vercel dashboard, go to Settings
2. Domains section
3. Add your domain
4. Follow DNS instructions
5. Takes 24 hours to activate

---

## Environment Variables on Deployment

### Important: Never Commit Secrets!

Your `.env.local` file should NEVER go to GitHub:

**Already ignored?** Check `.gitignore`:

```
.env.local  ← Should be here
```

If not, add it now!

### Set Variables on Platform

Each platform (Vercel, Netlify) has a way to set environment variables:

**Vercel:**
1. Dashboard → Project
2. Settings → Environment Variables
3. Add your keys

**Netlify:**
1. Site Settings
2. Build & Deploy → Environment
3. Add your keys

---

## Troubleshooting Deployments

### Issue: "Build Failed"

**What to do:**
1. Check build logs in dashboard
2. Look for error message
3. Fix in your code
4. Push to GitHub
5. Auto-redeploy

### Issue: "API returns 401"

**Likely cause:** Environment variables not set

**How to fix:**
1. Go to deployment platform settings
2. Check NEXT_PUBLIC_TMDB_API_KEY is set
3. Verify value is correct
4. Re-deploy

### Issue: "Blank page or 404"

**Likely cause:** Wrong build directory

**How to fix:**
1. Check build settings
2. For Next.js: should be `.next`
3. Check publish directory matches
4. Re-deploy

### Issue: "App works locally but not deployed"

**Common causes:**
- Missing environment variables
- Wrong build settings
- `.gitignore` excluded important files
- Case-sensitive path issues

**How to debug:**
1. Check build logs
2. Verify env variables
3. Check `.gitignore`
4. Test locally again

---

## After Deployment

### Celebrate! 🎉

You've published an app to the internet!

### Share Your Work

1. **Share the URL:**
   ```
   Check out my app: https://my-movie-app.vercel.app
   ```

2. **Add to Portfolio:**
   - GitHub profile
   - Portfolio website
   - Resume

3. **Social Media:**
   - Tweet about it
   - Post on LinkedIn
   - Share on Reddit

### Keep Improving

1. **Add features** from bonus list
2. **Fix bugs** users find
3. **Improve performance**
4. **Update design**

---

## Advanced: Manual Deployment

### Deploy Without GitHub

If you want to deploy without GitHub:

**Vercel CLI:**
```bash
# Install
npm install -g vercel

# Deploy
vercel

# Enter environment variables when prompted
```

**Netlify CLI:**
```bash
# Install
npm install -g netlify-cli

# Build
npm run build

# Deploy
netlify deploy --prod
```

---

## Performance Monitoring

### Check Your App Performance

**Vercel Analytics:**
1. Dashboard → Project
2. Analytics tab
3. See visit data, performance

**Web Vitals:**
1. Open your deployed app
2. F12 DevTools
3. Lighthouse tab
4. Run audit

### Optimize if Slow

1. Add image optimization
2. Use code splitting
3. Reduce bundle size
4. Cache static assets

---

## Continuous Integration/Deployment

### Automatic Testing & Deployment

When properly set up:

```
You push code → GitHub
    ↓
GitHub triggers Vercel
    ↓
Vercel runs tests
    ↓
Vercel deploys
    ↓
Your app updates
```

### Set Up (Advanced)

See GitHub Actions documentation to auto-test before deployment.

---

## Common Deployment Platforms Comparison

| Feature | Vercel | Netlify | Heroku |
|---------|--------|---------|--------|
| **Setup Time** | 2 min | 5 min | 10 min |
| **Free Tier** | Yes (generous) | Yes | No (paid) |
| **Next.js** | Best | OK | OK |
| **SSL/HTTPS** | Included | Included | Included |
| **Custom Domain** | Yes | Yes | Yes |
| **Build Speed** | Fast | Medium | Slow |
| **Best For** | Next.js | Static sites | Custom apps |

---

## Security Notes

### Keep API Keys Safe

- ✅ Use `.env.local` locally
- ✅ Set env vars on platform
- ✅ Never commit `.env.local`
- ❌ Never share API keys
- ❌ Never put secrets in code

### Prevent Abuse

- Set TMDB API rate limits
- Monitor usage
- Report abuse if needed

---

## Getting Your App URL

### Share This

After deployment, you have a URL:

```
https://movie-explorer-abc123.vercel.app
```

**Share with:**
- Friends & family
- Portfolio
- Social media
- Job applications
- GitHub profile

**Example:**
```
🎬 Check out my first web app!
I built a movie search app with React/Next.js
Live demo: https://movie-explorer-abc123.vercel.app
GitHub: https://github.com/me/movie-explorer-learning
```

---

## Congratulations!

You've deployed a production web app! 🚀

This is a **real accomplishment**. You:
- Built a full-stack app
- Used modern technologies
- Put it on the internet
- Made it production-ready

**You're officially a developer!** 👨‍💻

---

## Next Steps

1. **Share your app** - Get feedback
2. **Add more features** - Keep improving
3. **Optimize performance** - Make it faster
4. **Write blog post** - Share what you learned
5. **Build another app** - Apply what you learned

---

## Resources

- [Vercel Docs](https://vercel.com/docs)
- [Netlify Docs](https://docs.netlify.com)
- [Next.js Deployment](https://nextjs.org/docs/deployment)
- [Web Performance](https://web.dev/performance)

---

**Your app is live! 🌍**

Happy deploying!