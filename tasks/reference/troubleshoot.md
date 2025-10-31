# 🆘 Troubleshooting Guide

When something goes wrong, **don't panic!** This guide helps you fix common issues.

---

## Table of Contents

- [General Debugging Tips](#general-debugging-tips)
- [Setup & Installation Issues](#setup--installation-issues)
- [Running the Project](#running-the-project)
- [API & Fetching Issues](#api--fetching-issues)
- [Display & Styling Issues](#display--styling-issues)
- [Favorites & localStorage Issues](#favorites--localstorage-issues)
- [Git & GitHub Issues](#git--github-issues)
- [Performance Issues](#performance-issues)

---

## General Debugging Tips

### Rule #1: Read Error Messages Carefully

**Most Errors Tell You Exactly What's Wrong!**

When you see an error:
1. **Read it completely** (don't just see "Error" and panic)
2. **Look for line numbers** - tells you WHERE the problem is
3. **Look for the actual message** - tells you WHAT is wrong
4. **Try to understand it** - it's usually clear in English

Example error:
```
Error: Cannot find module 'axios'
```

This tells you: "You're trying to use a module called 'axios' that isn't installed"

Solution: `bun install axios`

### Rule #2: Check Browser Console

The browser console shows errors that happen in your code:

**How to open:**
- Windows/Linux: Press F12
- Mac: Press Cmd + Option + I

**Look for:**
- Red text = Errors (fix these!)
- Yellow text = Warnings (usually OK)
- Blue text = Logs (your debug info)

### Rule #3: Check Server Console

The terminal where you ran `bun dev` also shows errors:

```bash
$ bun dev

> next dev --port 3000
  ▲ Next.js 15.1.3

✓ Ready in 1.5s

⚠  Error: Cannot find module...  ← Error appears here
```

### Rule #4: Google the Error

Copy the error message and google it:

```
"Cannot find module 'shadcn/ui'" next.js
```

Stack Overflow usually has the answer!

### Rule #5: Try These First

Before digging deeper, try:

1. **Save your file** - Sometimes editors don't save automatically
2. **Restart dev server** - Ctrl+C then `bun dev`
3. **Clear node_modules** - `rm -rf node_modules && bun install`
4. **Clear cache** - `rm -rf .next && bun dev`
5. **Restart terminal** - Close and open new terminal

---

## Setup & Installation Issues

### Issue: "node: command not found"

**What it means:** Node.js isn't installed

**How to fix:**
1. Go to https://nodejs.org
2. Download LTS version
3. Install it
4. Restart your terminal completely
5. Try `node --version` again

---

### Issue: "bun: command not found"

**What it means:** Bun isn't installed

**How to fix:**
1. Install Bun:
   ```bash
   curl -fsSL https://bun.sh/install | bash
   ```
2. Restart terminal completely
3. Try `bun --version`

**If it still doesn't work:**
- Check Bun installation path
- Try downloading directly from https://bun.sh/download
- On Windows, ensure it's in your PATH

---

### Issue: "Permission denied" when installing

**What it means:** You don't have permission to install globally

**How to fix (Mac/Linux):**
```bash
sudo bun install
```

Enter your password when prompted.

**How to fix (Windows):**
- Run terminal as Administrator
- Right-click terminal → "Run as administrator"

---

### Issue: Version conflicts

**What it means:** Tools have incompatible versions

**How to fix:**
1. Check versions:
   ```bash
   node --version
   bun --version
   ```

2. Required versions:
   - Node: 20+
   - Bun: 1.1+

3. If versions are old, update them

---

## Running the Project

### Issue: "Port 3000 already in use"

**What it means:** Another program is using port 3000

**Error looks like:**
```
Error: listen EADDRINUSE: address already in use :::3000
```

**How to fix:**

**Option 1: Use different port**
```bash
bun dev --port 3001
```

**Option 2: Kill the process using port 3000**

Mac/Linux:
```bash
lsof -ti:3000 | xargs kill -9
```

Windows:
```bash
netstat -ano | findstr :3000
taskkill /PID <PID> /F
```

(Replace `<PID>` with the number shown)

---

### Issue: "Cannot find next.config.js"

**What it means:** Next.js project isn't set up

**How to fix:**
1. Make sure you're in project folder:
   ```bash
   cd movie-explorer-learning
   ```

2. Initialize Next.js:
   ```bash
   bun create next-app@latest .
   ```

3. Answer prompts with setup options

---

### Issue: "Module not found: can't resolve '@/components/ui/input'"

**What it means:** shadcn component not installed

**How to fix:**
```bash
bunx shadcn-ui@latest add input
```

**Which components to add:**
```bash
bunx shadcn-ui@latest add button
bunx shadcn-ui@latest add card
bunx shadcn-ui@latest add badge
bunx shadcn-ui@latest add skeleton
bunx shadcn-ui@latest add alert
```

---

### Issue: Dev server keeps crashing

**Error looks like:**
```
TypeError: Cannot read property 'map' of undefined
```

**How to fix:**
1. Save your file
2. Restart dev server: Ctrl+C then `bun dev`
3. Check for syntax errors (red underlines in editor)
4. Verify you didn't delete important code

---

## API & Fetching Issues

### Issue: 401 Unauthorized from TMDB API

**What it means:** API key is wrong or missing

**How to fix:**

1. Check `.env.local` exists in project root:
   ```
   NEXT_PUBLIC_TMDB_API_KEY=your_key_here
   NEXT_PUBLIC_TMDB_BASE_URL=https://api.themoviedb.org/3
   ```

2. Verify API key is correct:
   - Go to https://www.themoviedb.org/settings/api
   - Copy the exact key
   - Paste into `.env.local`

3. Check for typos (case-sensitive!)

4. Restart dev server after changing `.env.local`

5. Test in browser:
   ```
   http://localhost:3000/api/search?q=batman
   ```

Should return JSON with movies, not error.

---

### Issue: "Failed to fetch from TMDB" error

**What it means:** API request failed

**Reasons it happens:**
- API key is wrong
- TMDB is down
- No internet connection
- URL is malformed

**How to fix:**
1. Check your internet connection
2. Try a different search term
3. Verify API key is correct
4. Check TMDB status at https://status.themoviedb.org/
5. Test API directly: `http://localhost:3000/api/search?q=matrix`

---

### Issue: API returns empty results

**What it means:** Search term found no movies

**This is normal!** Not every search has results.

**How to fix:**
Try a popular movie:
- batman
- spider-man
- the matrix
- inception
- avatar

If popular movies work but your search doesn't, the title might be spelled differently.

---

### Issue: Images not loading (blank spaces)

**What it means:** Images failed to download

**Reasons:**
- Image URL is broken
- Image takes time to load
- Movie doesn't have poster
- Network issue

**How to fix:**
1. Wait longer - images load slowly sometimes
2. Check network tab (F12) → Images
3. Some movies don't have posters (normal!)
4. Check your internet connection

---

### Issue: "Uncaught TypeError: Cannot read property 'results' of undefined"

**What it means:** API response doesn't have expected data

**How to fix:**
1. Check API response in browser:
   ```
   http://localhost:3000/api/search?q=batman
   ```

2. Verify response has `{ results: [...] }`

3. Check your API route returns correct format:
   ```javascript
   return Response.json(data)  // Should work
   return Response.json({ results: data.results })  // More explicit
   ```

---

## Display & Styling Issues

### Issue: Layout looks broken on mobile

**What it means:** Responsive design not working

**How to fix:**
1. Open DevTools (F12)
2. Click mobile icon (top left)
3. Resize to different widths
4. Check that layout adapts

If it doesn't:
- Check Tailwind classes have responsive prefixes
- Example: `md:grid-cols-2` (2 columns on medium+)

---

### Issue: Cards too small/too large

**What it means:** Sizing classes wrong

**How to fix:**
Update card sizing in your component:

```javascript
// Instead of:
<div className="h-64 w-80">

// Try:
<div className="w-full h-auto">
```

---

### Issue: Text overlapping or unreadable

**What it means:** Font size or spacing issues

**How to fix:**
1. Check spacing classes (p-, m-, gap-)
2. Check font sizes (text-sm, text-lg, etc.)
3. Make sure text has enough padding
4. Example fix:
   ```javascript
   <div className="p-4">  ← Add padding
     <p className="text-lg">  ← Make text bigger
       {text}
     </p>
   </div>
   ```

---

### Issue: Colors look wrong

**What it means:** Tailwind color classes wrong

**How to fix:**
Check Tailwind color names:
```
bg-gray-900 ✓ Correct
bg-dark ✗ Wrong (should be bg-gray-900)

text-white ✓ Correct
text-light ✗ Wrong (should be text-white)
```

Full list: https://tailwindcss.com/docs/customizing-colors

---

## Favorites & localStorage Issues

### Issue: Favorites disappear after refresh

**What it means:** Data not saving to localStorage

**How to fix:**

1. Check browser console (F12) for errors
2. Verify `useFavorites` hook loads data:
   ```javascript
   useEffect(() => {
     const stored = localStorage.getItem('movieFavorites')
     // ... rest of code
   }, [])
   ```

3. Check localStorage isn't full:
   - F12 → Application → Local Storage
   - Look for 'movieFavorites' key

4. Try clearing and retesting:
   - F12 → Application → Local Storage
   - Delete 'movieFavorites' entry
   - Try adding favorite again

---

### Issue: "localStorage is not defined"

**What it means:** Tried to use localStorage in server component

**How to fix:**
Make sure component has `'use client'` at top:

```javascript
'use client'  ← Add this!

import { useEffect } from 'react'

export function MyComponent() {
  useEffect(() => {
    localStorage.getItem('key')  // Now it works
  }, [])
}
```

---

### Issue: Favorites button doesn't work

**What it means:** Click handler not firing

**How to fix:**
1. Check button has onClick:
   ```javascript
   <button onClick={() => addFavorite(movie)}>
     Add to Favorites
   </button>
   ```

2. Check useFavorites hook is working:
   - Open DevTools (F12)
   - Type in console: `localStorage.getItem('movieFavorites')`
   - Should show your favorites

3. Check component is client component:
   ```javascript
   'use client'  ← Required!
   ```

---

## Git & GitHub Issues

### Issue: "Permission denied (publickey)"

**What it means:** Git can't authenticate with GitHub

**How to fix:**

1. Set up SSH key:
   ```bash
   ssh-keygen -t ed25519 -C "your.email@example.com"
   ```

2. Add key to GitHub:
   - Go to https://github.com/settings/keys
   - Click "New SSH key"
   - Paste your public key

3. Try cloning again

---

### Issue: "fatal: not a git repository"

**What it means:** You're not in a git folder

**How to fix:**
1. Check you're in right folder:
   ```bash
   pwd  # Mac/Linux - shows current folder
   cd movie-explorer-learning
   ```

2. Make sure `.git` folder exists:
   ```bash
   ls -la  # Mac/Linux
   dir    # Windows
   ```

3. If no `.git`, initialize:
   ```bash
   git init
   ```

---

### Issue: "Changes not staged for commit"

**What it means:** You modified files but didn't stage them

**How to fix:**
```bash
# See what changed
git status

# Stage all changes
git add .

# Commit with message
git commit -m "My changes"

# Push to GitHub
git push
```

---

### Issue: "Remote repository not found"

**What it means:** Git can't find your GitHub repo

**How to fix:**
1. Check remote is correct:
   ```bash
   git remote -v
   ```

2. Should show your GitHub URL

3. If wrong, update it:
   ```bash
   git remote remove origin
   git remote add origin https://github.com/YOUR_USERNAME/movie-explorer-learning.git
   ```

4. Push again:
   ```bash
   git push -u origin main
   ```

---

## Performance Issues

### Issue: App is slow

**What it means:** Something is making it sluggish

**How to fix:**

1. Check network speed (F12 → Network)
   - Images taking long to load? Normal!
   - API taking long? Check internet

2. Check DevTools Performance tab
   - See what's taking time

3. Common causes:
   - Too many movie cards rendering
   - Images not optimized
   - Component re-rendering too much

4. Solution:
   - Use React.memo() for cards
   - Lazy load images
   - Reduce animations

---

### Issue: API calls taking forever

**What it means:** TMDB API is slow

**This is usually normal!** TMDB is a free service.

**How to check:**
1. Open DevTools (F12)
2. Go to Network tab
3. Do a search
4. Look at API request time
5. If >3 seconds, it's the API, not your code

**What you can do:**
- Add timeout logic
- Show loading state (you already do!)
- Cache results

---

### Issue: Page is unresponsive after search

**What it means:** Too much processing happening

**How to fix:**
1. Add loading state (you have this!)
2. Show skeleton loaders while fetching
3. Limit results displayed
4. Use pagination

---

## Still Stuck?

### When nothing works:

1. **Read the error message again** - 90% of the time it tells you the answer
2. **Google the exact error** - Stack Overflow probably has answer
3. **Check the code** - Usually syntax error nearby
4. **Try the nuclear option**:
   ```bash
   rm -rf node_modules
   bun install
   bun dev
   ```

5. **Ask for help** with:
   - Exact error message
   - What you did before error
   - Code relevant to error
   - What you've already tried

---

## Resources

- [MDN Web Docs](https://developer.mozilla.org) - JavaScript/Web API docs
- [Stack Overflow](https://stackoverflow.com) - Q&A for coding
- [Next.js Discord](https://discord.gg/nextjs) - Community help
- [TMDB API Docs](https://www.themoviedb.org/settings/api) - API documentation

---

## Remember

**Every error is a learning opportunity!** When something breaks:

1. Don't panic - it's fixable
2. Read the error - it helps
3. Search for solution - others had it too
4. Learn from it - now you know for next time
5. Keep going - you've got this!

---

**Still confused? Check PRD.md or README.md for more context!**

Happy debugging! 🔍