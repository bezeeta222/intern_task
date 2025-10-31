# Reference: Troubleshooting & Help

When something breaks or you get an error, this is your go-to section!

---

## What's in This Section

- **[troubleshoot.md](./troubleshoot.md)** - Comprehensive troubleshooting guide for all common issues

---

## Using This Guide

### When to Check Here

You should check this guide when:

- ❌ You get an error message
- ❌ Something isn't working as expected
- ❌ Your app crashes
- ❌ Code won't run
- ❌ Features aren't functioning
- ❌ You're confused about what went wrong

---

## How to Use troubleshoot.md

### Step 1: Find Your Issue
The guide is organized by topic:

- General Debugging Tips
- Setup & Installation Issues
- Running the Project
- API & Fetching Issues
- Display & Styling Issues
- Favorites & localStorage Issues
- Git & GitHub Issues
- Performance Issues

### Step 2: Look Up Your Specific Problem
Each section has common issues and solutions.

### Step 3: Follow the Solution
Most solutions are step-by-step.

### Step 4: Test
Verify your fix worked.

---

## Error Message? Don't Panic!

### The Golden Rule

**Most error messages tell you exactly what's wrong.**

When you see an error:

1. 📖 **Read it completely** - Don't just see "Error" and give up
2. 🔍 **Look for line numbers** - Shows WHERE the problem is
3. 💬 **Read the message** - Shows WHAT is wrong
4. 🧠 **Try to understand it** - It's usually written in English

### Example

```
Error: Cannot find module 'axios'
```

This tells you: "You're trying to use something called 'axios' that isn't installed"

**Solution**: `bun install axios`

---

## General Debugging Tips

From the troubleshooting guide, remember:

### Rule #1: Read Error Messages
- Copy the full error message
- Search it on Google
- Stack Overflow usually has an answer

### Rule #2: Check Browser Console
- Press F12 to open DevTools
- Red text = Errors (fix these!)
- Yellow text = Warnings (usually OK)

### Rule #3: Check Server Console
- The terminal where you ran `bun dev` also shows errors
- Pay attention to what it says

### Rule #4: Try These First
1. Save your file
2. Restart dev server (Ctrl+C then `bun dev`)
3. Clear cache (`rm -rf .next && bun dev`)
4. Restart terminal completely

---

## Common Issues Quick Links

### Setup Issues
- Node.js not found
- Bun not found
- Permission denied
- Version conflicts

→ See [troubleshoot.md - Setup & Installation Issues](./troubleshoot.md#setup--installation-issues)

### Running Your Project
- Port 3000 already in use
- Cannot find next.config.js
- Module not found
- Dev server crashes

→ See [troubleshoot.md - Running the Project](./troubleshoot.md#running-the-project)

### API Issues
- 401 Unauthorized from TMDB
- Failed to fetch
- Empty results
- Images not loading

→ See [troubleshoot.md - API & Fetching Issues](./troubleshoot.md#api--fetching-issues)

### Display Issues
- Layout broken on mobile
- Cards too small/large
- Text overlapping
- Colors look wrong

→ See [troubleshoot.md - Display & Styling Issues](./troubleshoot.md#display--styling-issues)

### Data Issues
- Favorites disappear after refresh
- localStorage not defined
- Favorites button doesn't work

→ See [troubleshoot.md - Favorites & localStorage Issues](./troubleshoot.md#favorites--localstorage-issues)

### Git Issues
- Permission denied (publickey)
- Not a git repository
- Changes not staged
- Remote not found

→ See [troubleshoot.md - Git & GitHub Issues](./troubleshoot.md#git--github-issues)

---

## How to Ask for Help

If you can't find your answer:

### Include in Your Question

1. **The exact error message** - Copy and paste it
2. **What you were trying to do** - Step by step
3. **Relevant code** - Show what's not working
4. **What you've already tried** - Don't waste time on repeated solutions
5. **Expected vs Actual** - What should happen vs what's happening

### Where to Ask

- **Stack Overflow** - For specific errors
- **GitHub Discussions** - For library-specific questions
- **Reddit r/learnprogramming** - For advice
- **Discord Communities** - For quick help (see resources)

---

## The Nuclear Option

If absolutely nothing works:

```bash
# Clear everything and reinstall
rm -rf node_modules .next
bun install
bun dev
```

This usually fixes weird issues that don't make sense.

---

## Key Rules to Remember

### Rule 1: Read Before Searching
- Read error message first
- Usually tells you what's wrong

### Rule 2: Search Your Error
- Copy error message
- Google it
- Stack Overflow has almost everything

### Rule 3: Check Your Code
- Look for typos
- Check syntax highlighting (red = bad)
- Compare with examples

### Rule 4: Restart Things
- Restart dev server
- Restart terminal
- Restart browser
- Often magically fixes things!

### Rule 5: Ask For Help
- Share your code
- Share your error
- Share what you tried
- Communities are helpful!

---

## Still Stuck After All This?

1. **Go back and read the error again** - You probably missed something
2. **Check [troubleshoot.md](./troubleshoot.md)** - Scroll through for similar issues
3. **Google the exact error** - Someone else had it
4. **Ask in community** - With code examples
5. **Walk away and come back** - Fresh eyes help!

---

## Remember

**Every programmer gets stuck.** Even experts!

Errors are:
- ✅ Normal and expected
- ✅ Learning opportunities
- ✅ Usually fixable with patience
- ✅ Confidence builders (you fixed it!)

You've got this! 💪

---

## Quick Links

- 📖 Full guide: [troubleshoot.md](./troubleshoot.md)
- 💬 Need resources? → [02-learning-concepts/resources.md](../02-learning-concepts/resources.md)
- 🆘 Setup help? → [01-getting-started/setup.md](../01-getting-started/setup.md)
- 🏗️ Building help? → [03-building-your-app/README.md](../03-building-your-app/)

---

**Open [troubleshoot.md](./troubleshoot.md) to find your answer! 🔧**
