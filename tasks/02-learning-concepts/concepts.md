# 📚 Key Concepts Explained

This document explains important concepts you'll learn during this project.

---

## Table of Contents

- [What is a Web Application?](#what-is-a-web-application)
- [Frontend vs Backend](#frontend-vs-backend)
- [React & Components](#react--components)
- [Hooks (useState, useEffect)](#hooks-usestate-useeffect)
- [API & API Routes](#api--api-routes)
- [Data Persistence](#data-persistence)
- [Responsive Design](#responsive-design)

---

## What is a Web Application?

### Simple Explanation

A web app is a program that runs in your browser.

**Examples:**
- Gmail (email)
- Netflix (movies)
- Twitter (social media)
- Your movie search app (search movies)

### How It's Different from Desktop Apps

**Desktop App:**
- Installed on your computer
- Stored on hard drive
- Updated through your computer
- Examples: Word, Photoshop, Chrome

**Web App:**
- Runs in browser (Firefox, Chrome, Safari)
- Stored online (server)
- Always newest version
- Works on phone, tablet, desktop

### Parts of a Web App

Every web app has these parts:

```
┌─────────────────────────────────────┐
│  Frontend (User Interface)          │
│  - What users see                   │
│  - Buttons, forms, text             │
│  - Runs in browser                  │
└─────────────────────────────────────┘
              ↕ (Communication)
┌─────────────────────────────────────┐
│  Backend (Server/Logic)             │
│  - What users don't see             │
│  - Processing data                  │
│  - Storing information              │
│  - Runs on a computer (server)      │
└─────────────────────────────────────┘
              ↕ (Data)
┌─────────────────────────────────────┐
│  Database (Storage)                 │
│  - Where data lives permanently     │
│  - Movies, users, favorites         │
│  - Like a filing cabinet            │
└─────────────────────────────────────┘
```

### Your Movie App

```
Frontend (React/Next.js)
- Search box you type into
- Movie cards displayed
- Favorites button
- Runs in YOUR browser

       ↓

Backend (Next.js API Routes)
- Receives your search
- Calls TMDB API
- Sends back results
- Keeps API key secret

       ↓

TMDB API (External Service)
- Has movie database
- Sends movie data
- Used by Netflix, IMDb, etc.
```

---

## Frontend vs Backend

### Frontend

**What is it?**
Everything the user sees and interacts with.

**Technology:**
- HTML (structure)
- CSS (styling)
- JavaScript/React (interactivity)

**Runs where?**
- User's browser
- On their computer/phone

**You can see:**
- Inspector tool (F12)
- Source code
- Network requests

**Example:** Netflix homepage
- What you see: Title, search box, movie tiles
- What you do: Click to watch movie

### Backend

**What is it?**
The "engine" that makes things work behind the scenes.

**Technology:**
- Node.js (JavaScript on server)
- Python, Java, C# (other options)
- Databases (PostgreSQL, MongoDB)

**Runs where?**
- On a server (computer that's always on)
- Not on user's computer

**Users CAN'T see:**
- Actual code
- Database queries
- API keys

**Example:** Netflix watching a movie
- Backend checks: Is user logged in?
- Backend checks: Does user have subscription?
- Backend streams the video
- Backend logs watch history
- User just sees it playing

### Communication

Frontend and Backend talk to each other:

```
Frontend says: "User searched for 'batman'"
    ↓
Backend processes search
    ↓
Backend says: "Here are 50 Batman movies"
    ↓
Frontend displays results
```

This communication uses APIs (see below).

---

## React & Components

### What is React?

React is a JavaScript library for building user interfaces.

**Why use it?**
- Makes building UIs easier
- Reusable pieces (components)
- Automatically updates UI
- Very popular (used by Netflix, Facebook, Uber)

### What are Components?

A component is a reusable piece of UI.

Think of it like LEGO:
- LEGO brick = component
- You can use bricks multiple times
- Different combinations = different builds
- Your app = combination of components

**Components in your app:**
- SearchBar (input + button)
- MovieCard (poster + title + rating)
- MovieGrid (many cards)
- Header (title + nav)

### Component Structure

```javascript
// This is a component
function MyComponent() {
  return (
    <div>
      <h1>Hello!</h1>
      <button>Click me</button>
    </div>
  )
}

// You can use it multiple times:
<MyComponent />
<MyComponent />
<MyComponent />

// Result: Three identical "Hello!" boxes
```

### Components with Props

Components can accept data (props):

```javascript
// Component definition
function MovieCard({ title, rating }) {
  return (
    <div>
      <h2>{title}</h2>
      <p>Rating: {rating}</p>
    </div>
  )
}

// Using component with different data:
<MovieCard title="Batman" rating={8.5} />
<MovieCard title="Superman" rating={7.2} />
<MovieCard title="Spider-Man" rating={8.1} />

// Result: Three cards with different movies
```

---

## Hooks (useState, useEffect)

### What are Hooks?

Hooks are functions that "hook into" React features.

### useState Hook

**What it does:** Lets you store and change data

**When to use:** When something changes based on user action

```javascript
// Example: Counter button
import { useState } from 'react'

function Counter() {
  const [count, setCount] = useState(0)
  // count = current value (starts at 0)
  // setCount = function to change the value

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  )
}

// User sees: Count: 0 [Increment Button]
// User clicks button
// useState updates count to 1
// Component re-renders
// User sees: Count: 1 [Increment Button]
```

**Your movie app uses useState for:**
- Storing search query
- Storing search results
- Tracking loading state
- Tracking error message

### useEffect Hook

**What it does:** Runs code at specific times

**When to use:** When you need to do something after rendering

```javascript
// Example: Load favorites when page loads
import { useState, useEffect } from 'react'

function FavoritesPage() {
  const [favorites, setFavorites] = useState([])

  useEffect(() => {
    // This code runs ONCE when page first loads
    const saved = localStorage.getItem('movieFavorites')
    if (saved) {
      setFavorites(JSON.parse(saved))
    }
  }, [])
  // [] = "run only once on mount"

  return <div>{/* render favorites */}</div>
}
```

**Your movie app uses useEffect for:**
- Loading favorites from localStorage
- Saving favorites when they change
- Setting up API requests

### useState vs useEffect

**useState:**
- Tracks data that changes
- Like a variable that re-renders when it changes
- Updates immediately

**useEffect:**
- Runs code at specific times
- Like a timer or event listener
- Runs after render

---

## API & API Routes

### What is an API?

API = "Application Programming Interface"

**Simple explanation:**
An API lets two apps talk to each other.

**Restaurant analogy:**
- You (frontend) = customer
- Restaurant kitchen (backend) = your server
- Waiter = API
- Menu = API documentation
- Your request = API request
- Cooked food = API response

### How Your App Uses APIs

```
Your App (Frontend)
    ↓ (sends request)
"Get movies where title contains 'batman'"
    ↓
Your Server (Backend API Route)
    ↓ (takes request)
"OK, let me ask TMDB..."
    ↓
TMDB API
    ↓ (sends response)
{ results: [ { title: "Batman", ... }, ... ] }
    ↓
Your Server
    ↓ (sends response back)
```

### API Request/Response

**Request (what your app sends):**
```
GET /api/search?q=batman

Means: "Hey, search for movies with query 'batman'"
```

**Response (what server sends back):**
```json
{
  "results": [
    {
      "id": 272,
      "title": "Batman",
      "vote_average": 7.5,
      "poster_path": "/...",
      ...
    },
    ...
  ]
}
```

### API Routes in Next.js

An API route is code that runs on YOUR server:

```javascript
// File: app/api/search/route.js

export async function GET(request) {
  // This code runs on YOUR SERVER
  // Not in user's browser!
  
  // Get search query
  const query = request.nextUrl.searchParams.get('q')
  
  // Call TMDB API (with your secret key)
  const response = await fetch(
    `https://api.themoviedb.org/3/search/movie?query=${query}&api_key=${SECRET_KEY}`
  )
  
  // Return results
  const data = await response.json()
  return Response.json(data)
}
```

**Why have an API route?**

**Bad way (frontend calls TMDB directly):**
```
Frontend → TMDB (API key visible in browser!) ❌
```
Anyone can see your secret API key!

**Good way (use API route):**
```
Frontend → Your Server → TMDB (key hidden!) ✅
```
Only your server knows the secret key!

---

## Data Persistence

### What is Data Persistence?

Persistence = Data stays even after you close the app.

**Without persistence:**
- Save movie to favorites
- Close browser
- Reopen
- Favorite is gone ❌

**With persistence:**
- Save movie to favorites
- Close browser
- Reopen
- Favorite is still there ✅

### Ways to Persist Data

**1. localStorage (Browser Storage)**

Stores data in the browser:
```javascript
// Save
localStorage.setItem('key', 'value')

// Get
const value = localStorage.getItem('key')

// Delete
localStorage.removeItem('key')
```

**Pros:**
- Free
- No server needed
- Works immediately
- Persists forever

**Cons:**
- Limited size (~5MB)
- Only stores strings
- No backup

**Use for:**
- Favorites list
- User preferences
- Temporary data

**2. Cookies**

Stores small data in cookies:
```javascript
// Set
document.cookie = "name=value"

// Get (more complex)
const cookies = document.cookie.split(';')
```

**Pros:**
- Sent with every request
- Can set expiration

**Cons:**
- Limited size
- Security concerns
- Complex to use

**3. Database (Backend)**

Stores data on server:
```javascript
// In backend
const favorite = {
  userId: 123,
  movieId: 456,
  savedAt: new Date()
}
database.save(favorite)
```

**Pros:**
- Unlimited size
- Secure
- Syncs across devices
- Backup protection

**Cons:**
- Needs backend setup
- More complex
- Costs money for large scale

**Your app uses localStorage:**
- Stores favorites locally
- Persists after refresh
- Perfect for learning project

---

## Responsive Design

### What is Responsive Design?

Responsive = Design adapts to different screen sizes.

**Without responsive:**
- Works great on desktop
- Broken on mobile
- Text unreadable on phone

**With responsive:**
- Looks good on all devices
- Desktop view = 3 columns
- Tablet view = 2 columns
- Mobile view = 1 column

### Screen Sizes

```
Mobile:   < 640px   (phones)
Tablet:   640-1024px (iPad)
Desktop:  > 1024px   (laptop/desktop)
```

### How to Make Responsive UI

**Method 1: CSS Media Queries**
```css
/* Desktop */
.grid { display: grid; grid-template-columns: 1fr 1fr 1fr; }

/* Tablet */
@media (max-width: 1024px) {
  .grid { grid-template-columns: 1fr 1fr; }
}

/* Mobile */
@media (max-width: 640px) {
  .grid { grid-template-columns: 1fr; }
}
```

**Method 2: Tailwind CSS (what you'll use)**
```javascript
<div className="grid-cols-1 md:grid-cols-2 lg:grid-cols-3">
  {/* 1 column on mobile */}
  {/* 2 columns on tablets (md) */}
  {/* 3 columns on desktop (lg) */}
</div>
```

### Testing Responsive Design

**In browser (F12):**
1. Press F12 to open DevTools
2. Click mobile icon (top left)
3. Choose device or resize

**Real device:**
1. Run dev server
2. Find your computer's IP address
3. Visit `http://192.168.x.x:3000` on phone
4. Test on actual device

---

## Summary

**Web App = Frontend (UI) + Backend (Logic) + Database (Storage)**

**React** = Framework for building UIs

**Hooks** = useState (store data), useEffect (run code)

**APIs** = Way for apps to communicate

**Persistence** = Data that survives browser refresh

**Responsive** = Looks good on all devices

---

---

## Next Steps

Now that you understand the concepts, you're ready to code!

**What to do next:**
1. Go back to [README.md](./README.md) if you need guidance
2. Follow the recommended learning path
3. Wait for `PRD.md` (project requirements document) from your instructor
4. If you get stuck, check [troubleshoot.md](./troubleshoot.md)

Remember: Understanding these concepts takes time. Don't worry if you need to read this again!

**Happy learning! 🚀**