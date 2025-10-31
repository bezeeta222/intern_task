# 🚀 Web Development Learning Project - Movie Explorer

Welcome! This is a **complete beginner's guide** to learning web development. You'll build a real movie search app while learning fundamental programming concepts.

---

## 📚 What You'll Learn

By completing this project, you'll understand:

- **Frontend Development** - How to build user interfaces with React
- **Backend Development** - How to create API routes and handle requests
- **State Management** - How to store and update data in your app
- **APIs** - How different applications communicate
- **Deployment** - How to put your app on the internet
- **Git & GitHub** - How to version control and share your code

---

## 🎯 Quick Start (5 Steps)

1. **[Fork & Clone This Repo](#fork--clone-the-repository)** - Get the project on your computer
2. **[Setup Your Computer](./tasks/01-getting-started/)** - Install all necessary tools (Node.js, Bun, VS Code)
3. **[Understand Core Concepts](./tasks/02-learning-concepts/)** - Learn what frontend, backend, and APIs mean
4. **[Build Your App](./tasks/03-building-your-app/)** - Follow day-by-day instructions in PRD.md
5. **[Deploy Live](./tasks/04-deployment/)** - Put your app on the internet for everyone to see

---

## 🍴 Fork & Clone the Repository

### What is Forking?

**Forking** means creating your own copy of this project on GitHub. This lets you:
- Have your own version to work on
- Make changes without affecting the original
- Push your code to GitHub
- Show your work on your GitHub profile

### Step-by-Step: Fork & Clone

#### Step 1: Fork on GitHub

1. Go to the GitHub repository URL (ask your instructor for the link)
2. Click the **"Fork"** button (top right)
3. Choose where to fork (your personal GitHub account)
4. Wait for GitHub to create your copy
5. You now have your own version!

#### Step 2: Clone to Your Computer

Open terminal/command prompt and run:

```bash
# Clone YOUR forked repository (not the original!)
git clone https://github.com/YOUR_USERNAME/movie-explorer-learning.git

# Replace YOUR_USERNAME with your actual GitHub username!
# Example: git clone https://github.com/john-doe/movie-explorer-learning.git

# Enter the project folder
cd movie-explorer-learning
```

#### Step 3: Verify It Works

```bash
# Check that you're in the right folder
pwd  # on Mac/Linux
# or
cd   # on Windows - should show your folder path

# Check that git is set up
git status
# Should show: On branch main, nothing to commit
```

### Why Fork Instead of Just Download?

| Feature | Fork | Download |
|---------|------|----------|
| Your own copy | ✅ Yes | ✅ Yes |
| On GitHub | ✅ Yes | ❌ No |
| Can push code | ✅ Yes | ❌ No |
| Track changes | ✅ Yes | ❌ No |
| Share on portfolio | ✅ Yes | ❌ No |

**Use Fork** so you can push your progress to GitHub!

### Next Steps After Cloning

After you fork and clone:

```bash
# You're now in your project folder
movie-explorer-learning/

# Next: Follow setup guide
# Go to: tasks/01-getting-started/setup.md
```

---

## 📖 Documentation Structure

Everything is organized in the **`tasks/`** directory with clear sections:

### [📦 tasks/ Directory](./tasks/) - All Learning Content

#### [01 - Getting Started](./tasks/01-getting-started/)
- **[setup.md](./tasks/01-getting-started/setup.md)** - Install tools
  - Step-by-step for Windows, Mac, and Linux
  - Verification checklist to confirm everything works
  - **Time:** 30-45 minutes

#### [02 - Learning Concepts](./tasks/02-learning-concepts/)
- **[concepts.md](./tasks/02-learning-concepts/concepts.md)** - Core programming concepts
  - Web applications explained simply
  - Frontend vs Backend
  - React components and hooks
  - APIs and data persistence
  - **Time:** 1-2 hours reading + practice

- **[resources.md](./tasks/02-learning-concepts/resources.md)** - Curated learning materials
  - Official documentation links
  - Video tutorials for visual learners
  - Practice platforms to reinforce learning
  - Communities where you can get help

#### [03 - Building Your App](./tasks/03-building-your-app/)
- **[PRD.md](./tasks/03-building-your-app/PRD.md)** - Day-by-day project requirements
  - Day 1-5: Build the core app with full code examples
  - Each day has clear tasks, code snippets, and acceptance criteria
  - **Time:** 1-2 weeks (5-10 hours per week)

#### [04 - Deployment](./tasks/04-deployment/)
- **[deployment.md](./tasks/04-deployment/deployment.md)** - Put your app online
  - Step-by-step Vercel deployment
  - How to add environment variables
  - Custom domain setup
  - **Time:** 30 minutes

#### [Reference - Troubleshooting](./tasks/reference/)
- **[troubleshoot.md](./tasks/reference/troubleshoot.md)** - Fix common problems
  - Setup issues (Node.js not found, etc.)
  - Running the project (port errors, etc.)
  - API problems (401 errors, etc.)
  - Display issues (styling not working, etc.)
  - **When to use:** When you get an error message (use anytime!)

---

## 🛠️ What You'll Build

A **Movie Explorer App** that:

- ✅ Searches the TMDB movie database
- ✅ Displays movie results with posters and ratings
- ✅ Lets users favorite movies (saved locally)
- ✅ Works on desktop and mobile devices
- ✅ Deploys to the internet as a live website

**By the end, you'll have:**
- A live app you can share with friends
- Code on GitHub to show employers
- Real understanding of how web apps work
- Foundation skills for more advanced projects

---

## 🎓 Recommended Learning Path

### Week 1: Get Ready
- [ ] Complete [setup.md](./setup.md) - Get your computer ready
- [ ] Read [concepts.md](./concepts.md) - Understand the basics
- [ ] Have VS Code and terminal open and working

### Week 2-3: Build Your App
- [ ] Follow Day 1-3 in PRD.md - Create basic structure
- [ ] Follow Day 4-5 in PRD.md - Add search and display features
- [ ] Test your app locally
- [ ] Use [troubleshoot.md](./troubleshoot.md) when stuck

### Week 4: Polish & Deploy
- [ ] Add extra features (favorites, sorting, etc.)
- [ ] Test on mobile devices
- [ ] Follow [deployment.md](./deployment.md) - Deploy to Vercel
- [ ] Share your app!

---

## 📝 File Structure

```
intern/
├── README.md                           ← You are here! Main entry point
└── tasks/
    ├── README.md                       ← Overview of all sections
    ├── 01-getting-started/
    │   ├── README.md                   ← Section overview
    │   └── setup.md                    ← Install tools & verify
    ├── 02-learning-concepts/
    │   ├── README.md                   ← Section overview
    │   ├── concepts.md                 ← Learn fundamentals
    │   └── resources.md                ← Learning materials
    ├── 03-building-your-app/
    │   ├── README.md                   ← Section overview
    │   └── PRD.md                      ← Day-by-day tasks (coming soon)
    ├── 04-deployment/
    │   ├── README.md                   ← Section overview
    │   └── deployment.md               ← Deploy to internet
    └── reference/
        ├── README.md                   ← Help guide
        └── troubleshoot.md             ← Fix problems & debug
```

---

## ❓ When to Use Each File

| Situation | Read This |
|-----------|-----------|
| "I need to install tools on my computer" | [tasks/01-getting-started/setup.md](./tasks/01-getting-started/setup.md) |
| "I don't understand what Frontend/Backend means" | [tasks/02-learning-concepts/concepts.md](./tasks/02-learning-concepts/concepts.md) |
| "What should I code today?" | [tasks/03-building-your-app/](./tasks/03-building-your-app/) (waiting for PRD.md) |
| "I want to learn more about React/JavaScript" | [tasks/02-learning-concepts/resources.md](./tasks/02-learning-concepts/resources.md) |
| "My code broke and I don't know why" | [tasks/reference/troubleshoot.md](./tasks/reference/troubleshoot.md) |
| "I finished building - now what?" | [tasks/04-deployment/deployment.md](./tasks/04-deployment/deployment.md) |

---

## 🚦 Getting Started Right Now

### Option 1: Brand New (First Time)
If this is your **first time ever**:
1. Fork the repository on GitHub (see [Fork & Clone section](#fork--clone-the-repository) above)
2. Clone it to your computer
3. Go to [tasks/01-getting-started/](./tasks/01-getting-started/)
4. Follow [setup.md](./tasks/01-getting-started/setup.md) carefully
5. Run the verification checklist
6. Come back when everything works

### Option 2: Already Have Tools Set Up
If you **already have Node.js, Bun, and VS Code**:
1. Fork the repository on GitHub
2. Clone it to your computer
3. Go to [tasks/02-learning-concepts/](./tasks/02-learning-concepts/)
4. Read [concepts.md](./tasks/02-learning-concepts/concepts.md)
5. Then follow [tasks/03-building-your-app/PRD.md](./tasks/03-building-your-app/PRD.md)

### Option 3: Something's Wrong or Confused
If you're **getting errors or stuck**:
1. Go to [tasks/reference/](./tasks/reference/)
2. Check [troubleshoot.md](./tasks/reference/troubleshoot.md)
3. Search for your error message
4. Follow the solution steps

---

## 💡 Key Principles for This Learning Project

### 1. Learn By Doing
- Don't just read code - write it yourself
- Errors are learning opportunities
- Break things and fix them

### 2. Understand First, Code Second
- Read [concepts.md](./concepts.md) before coding
- Understand what each part does
- Then build it yourself

### 3. Test Everything
- Run your app frequently
- Check the browser console (F12)
- Make sure features work

### 4. Ask for Help
- Share your code on GitHub
- Ask questions in communities (see [resources.md](./resources.md))
- Share error messages completely

### 5. Celebrate Progress
- Finishing each day is a win
- Getting errors and fixing them is progress
- Deploying your app is huge!

---

## 🎯 Learning Objectives by Week

### After Week 1
You should understand:
- What frontend and backend are
- How React components work
- What an API is
- How to use your development tools

### After Week 2-3
You should be able to:
- Create React components
- Use React hooks (useState, useEffect)
- Make API requests
- Display data in your app

### After Week 4
You should have:
- A working movie search app
- Code on GitHub
- A deployed live website
- Understanding of full-stack web development

---

## 🔧 Technology Stack

You'll learn to use **professional, industry-standard technologies**:

### Frontend
- **React 18** - Modern UI library
- **Next.js 14** - Full-stack React framework
- **TypeScript** - Type-safe JavaScript (industry standard)
- **shadcn/ui** - Professional component library
- **Tailwind CSS** - Utility-first styling

### Backend & APIs
- **Next.js API Routes** - Serverless backend
- **TMDB API** - Movie database (real external API)

### Tools & Version Control
- **Bun** - Fast JavaScript runtime
- **Git** - Version control
- **GitHub** - Code hosting and collaboration
- **Vercel** - Production deployment

### Why This Stack?

✅ **TypeScript** - Catches bugs before they happen, used by 80% of professional teams
✅ **shadcn/ui** - Pre-built accessible components, saves development time
✅ **Next.js** - Full-stack capabilities, built-in optimization
✅ **Industry Standard** - These exact technologies are used at Netflix, Uber, Airbnb, and thousands of other companies

**This project teaches you to code like a professional developer.**

---

## 🆘 Stuck? Here's What to Do

### 1. Check Your Error Message
Most errors are descriptive. Read it carefully!

### 2. Search [troubleshoot.md](./troubleshoot.md)
Your problem probably has a solution there.

### 3. Google Your Error
Copy the error message and search for it. Stack Overflow probably has an answer.

### 4. Ask in Communities
Share your code and error on:
- Reddit: r/learnprogramming
- Discord: Next.js or React communities
- GitHub Discussions

**Always include:**
- The exact error message
- What you were trying to do
- What you've already tried

---

## 📚 Additional Resources

- **React Learning**: https://react.dev
- **Next.js Guide**: https://nextjs.org/docs
- **JavaScript Reference**: https://developer.mozilla.org/en-US/docs/Web/JavaScript
- **Tailwind CSS**: https://tailwindcss.com/docs
- **TMDB API Docs**: https://www.themoviedb.org/settings/api

See [resources.md](./resources.md) for many more learning materials.

---

## ⚡ Pro Tips for Success

1. **Code Along** - Don't just read, actually type the code
2. **Take Breaks** - Programming is mental work, rest helps
3. **Read Errors** - They tell you what's wrong
4. **Google First** - 90% of problems are already solved
5. **Build Projects** - The best way to learn
6. **Read Others' Code** - GitHub has great examples

---

## 🎬 Project Showcase

When you finish, you'll have:

```
✅ A movie search app
✅ Code on GitHub
✅ A live website (your-name.vercel.app)
✅ New skills to list on your resume
✅ Portfolio project to show employers
```

---

## 📞 Need Help?

### Quick Problems?
→ Check [tasks/reference/troubleshoot.md](./tasks/reference/troubleshoot.md)

### Want to Learn More?
→ See [tasks/02-learning-concepts/resources.md](./tasks/02-learning-concepts/resources.md)

### Don't Understand Concepts?
→ Read [tasks/02-learning-concepts/concepts.md](./tasks/02-learning-concepts/concepts.md) again, then Google the topic

### Ready to Code?
→ Open [tasks/03-building-your-app/PRD.md](./tasks/03-building-your-app/PRD.md) - Start with Day 1!

### Ready to Deploy?
→ Follow [deployment.md](./tasks/04-deployment/deployment.md)

---

## 🏁 Next Steps

1. **Right Now**:
   - Read this entire README
   - Understand the structure
   - Know where to find everything

2. **Before Anything Else**:
   - Fork the repository on GitHub
   - Clone to your computer
   - Verify git is working (`git status`)
   - See [Fork & Clone section](#fork--clone-the-repository) for details

3. **Next**:
   - Go to [tasks/01-getting-started/](./tasks/01-getting-started/)
   - Follow [setup.md](./tasks/01-getting-started/setup.md)
   - Set up your computer and tools

4. **Then**:
   - Go to [tasks/02-learning-concepts/](./tasks/02-learning-concepts/)
   - Read [concepts.md](./tasks/02-learning-concepts/concepts.md)
   - Learn the fundamentals

5. **Finally**:
   - Go to [tasks/03-building-your-app/](./tasks/03-building-your-app/)
   - Follow [PRD.md](./tasks/03-building-your-app/PRD.md)
   - Follow daily tasks to build your app (Day 1-5)

6. **When Done**:
   - Go to [tasks/04-deployment/](./tasks/04-deployment/)
   - Deploy your app to Vercel
   - Share your live app with the world!

---

## 💪 You've Got This!

Building web apps seems hard, but it's just **small steps done consistently**. Every developer started exactly where you are now.

**Remember:**
- You CAN do this
- Errors are learning
- Progress takes time
- You're building real skills

---

**Ready to start? Go to [setup.md](./setup.md) → Next step is setting up your computer! 🚀**

---

*Last Updated: October 31, 2025*
*For Beginners | Learning-Focused | Project-Based*
