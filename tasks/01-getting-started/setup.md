# 🔧 Complete Setup Guide for Beginners

This guide walks you through every step to get your development environment ready.

---

## Table of Contents

1. [System Requirements](#system-requirements)
2. [Install Node.js](#install-nodejs)
3. [Install Bun](#install-bun)
4. [Install VS Code](#install-vs-code)
5. [Git & GitHub Setup](#git--github-setup)
6. [TMDB API Key](#tmdb-api-key)
7. [Verification Checklist](#verification-checklist)

---

## System Requirements

### Computer

- **Windows** 10 or later
- **Mac** (Intel or Apple Silicon)
- **Linux** (Ubuntu, Fedora, etc.)

Any modern laptop will work. You don't need a powerful computer for development.

### Internet

Stable internet connection for:
- Downloading tools
- Using TMDB API
- GitHub access

### Storage

- ~500MB for Node.js and tools
- ~1GB for your project

---

## Install Node.js

### What is Node.js?

Node.js lets you run JavaScript on your computer (not just in browsers).

### Step-by-Step Installation

#### Mac (All)

**Option 1: Using Homebrew (Recommended)**

If you have Homebrew:
```bash
brew install node@20
```

If you don't have Homebrew:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**Option 2: Direct Download**

1. Go to https://nodejs.org
2. Click "LTS" (Long Term Support)
3. Download the Mac installer
4. Double-click and follow prompts
5. Accept all defaults

#### Windows

1. Go to https://nodejs.org
2. Click "LTS" version
3. Download Windows installer
4. Double-click the downloaded file
5. Click "Install"
6. Follow the setup wizard (accept defaults)
7. Click "Finish"

#### Linux (Ubuntu/Debian)

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### Verify Installation

Open terminal/command prompt and type:

```bash
node --version
```

You should see: `v20.x.x` or higher

If you see an error, Node.js didn't install correctly. Try again.

---

## Install Bun

### What is Bun?

Bun is a fast JavaScript runtime and package manager. It's like Node.js but faster.

### Installation

#### Mac & Linux

Copy and paste this entire command:

```bash
curl -fsSL https://bun.sh/install | bash
```

Then restart your terminal.

#### Windows

**Using PowerShell:**

```bash
iwr https://bun.sh/install.ps1 -useb | iex
```

Or download from https://bun.sh/download

### Verify Installation

```bash
bun --version
```

You should see: `1.1.x` or higher

### Note for Windows Users

If PowerShell command doesn't work:

1. Go to https://bun.sh/download
2. Download Windows build
3. Extract to your programs folder
4. Add to PATH (search "environment variables")

---

## Install VS Code

### What is VS Code?

VS Code is a code editor. It's what you'll use to write your code.

### Installation

1. Go to https://code.visualstudio.com
2. Click "Download"
3. Choose your operating system
4. Double-click installer
5. Follow prompts (accept all defaults)
6. Click "Finish"

### First Time Setup

1. Open VS Code
2. Click "Extensions" (left sidebar, looks like squares)
3. Search for "ES7+ React/Redux/React-Native snippets"
4. Click "Install" on the one by dsznajder
5. Done!

### Optional but Recommended Extensions

- **Prettier** (Code formatter)
- **ESLint** (Error checker)
- **Thunder Client** (Test APIs)

---

## Git & GitHub Setup

### What is Git?

Git is version control - tracks changes to your code.

### What is GitHub?

GitHub is where you store code online and collaborate with others.

### Install Git

#### Mac

Using Homebrew:
```bash
brew install git
```

Or download from https://git-scm.com

#### Windows

1. Go to https://git-scm.com
2. Download Windows version
3. Double-click installer
4. Accept all defaults
5. Click "Finish"

#### Linux

```bash
sudo apt-get install git
```

### Verify Git Installation

```bash
git --version
```

You should see: `git version 2.x.x`

### GitHub Account

1. Go to https://github.com
2. Click "Sign up"
3. Enter email address
4. Create password
5. Choose username
6. Verify email
7. Done!

### GitHub CLI (Optional but Recommended)

Install GitHub CLI to create repos from terminal:

**Mac:**
```bash
brew install gh
```

**Windows:**
```bash
choco install gh
```

(If you don't have choco, download from https://cli.github.com)

**Linux:**
```bash
sudo apt-get install gh
```

**Verify:**
```bash
gh --version
```

### Configure Git (Important!)

Tell Git who you are:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Replace with your real name and email!

---

## TMDB API Key

### What is TMDB?

The Movie Database (TMDB) is a free API that gives you movie data.

### Get Your API Key

1. Go to https://www.themoviedb.org/settings/api
2. Click "Create an API Key"
3. Select "Developer"
4. Agree to terms
5. Fill in application info:
   - **Name**: Movie Explorer Learning
   - **URL**: http://localhost:3000
   - **Summary**: Learning project for beginners
6. Click "Submit"
7. Accept terms again
8. You'll see your API key!

### Keep Your Key Safe

- **Never** share this key publicly
- **Never** put it on GitHub
- **Never** email it
- It's like a password!

---

## Verification Checklist

Before starting the project, verify everything works:

```bash
# Check Node.js
node --version
# Should show: v20.x.x or higher

# Check Bun
bun --version
# Should show: 1.1.x or higher

# Check Git
git --version
# Should show: git version 2.x.x

# Check GitHub (if installed)
gh --version
# Should show: gh version x.x.x
```

### System Check Script

Create a file `check-setup.sh` (Mac/Linux) or `check-setup.bat` (Windows) with:

**For Mac/Linux (`check-setup.sh`):**
```bash
#!/bin/bash
echo "=== System Check ==="
echo ""
echo "Node.js:"
node --version
echo ""
echo "Bun:"
bun --version
echo ""
echo "Git:"
git --version
echo ""
echo "Git User:"
git config user.name
git config user.email
echo ""
echo "=== All Good? ==="
```

**For Windows (`check-setup.bat`):**
```batch
@echo off
echo === System Check ===
echo.
echo Node.js:
node --version
echo.
echo Bun:
bun --version
echo.
echo Git:
git --version
echo.
echo === All Good? ===
```

---

## Troubleshooting Setup Issues

### Node.js Not Found

**Problem:** `command not found: node`

**Solutions:**
1. Node.js not installed - download from nodejs.org
2. Terminal not restarted after install - close and reopen
3. Path not set - reinstall and check "Add to PATH"

### Bun Not Found

**Problem:** `command not found: bun`

**Solutions:**
1. Not installed - run curl command again
2. Terminal not restarted - close and reopen terminal
3. Windows specific - ensure you installed correctly

### Git Not Found

**Problem:** `command not found: git`

**Solutions:**
1. Not installed - download from git-scm.com
2. Not in PATH - reinstall and check "Add Git to PATH"

### API Key Not Working

**Problem:** API returns 401 Unauthorized

**Solutions:**
1. Check API key is correct (copy from TMDB settings)
2. Make sure `.env.local` file exists
3. Restart dev server after adding `.env.local`
4. Check for typos in key

### Terminal Commands Not Working

**Problem:** Commands work on friend's computer but not yours

**Solutions:**
1. Make sure you're in right folder (`cd movie-explorer-learning`)
2. Restart terminal completely
3. Check that tools are installed (`node --version`)
4. Try a different terminal (PowerShell instead of CMD on Windows)

---

## Next Steps

Once all verified:

1. Fork this repo (see README.md)
2. Clone to your computer
3. Open in VS Code
4. Run Day 1 tasks from PRD.md

---

## Still Stuck?

Check these files:
- **README.md** - General overview
- **TROUBLESHOOTING.md** - Common problems
- **PRD.md** - Day-by-day tasks

Or Google: "How to install [tool name] on [your OS]"

---

**Setup complete! Time to build something amazing! 🚀**