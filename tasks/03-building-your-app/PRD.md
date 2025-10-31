# 🎬 Movie Explorer App - Product Requirements Document (PRD)

This document contains your day-by-day tasks for building the movie explorer app. Follow it exactly!

---

## 📖 Quick Overview

### What You're Building
A **Movie Search App** that:
- Searches TMDB movie database
- Displays results with posters and details
- Allows users to save favorite movies
- Works on mobile and desktop
- Is fully deployed online

### Timeline
- **Day 1-2**: Setup and search functionality (3-4 hours)
- **Day 3**: Display results beautifully (2-3 hours)
- **Day 4**: Add favorites feature (2-3 hours)
- **Day 5**: Polish and optimize (2-3 hours)

**Total: 1-2 weeks of work (5-10 hours per week)**

---

## ✅ Prerequisites

Before starting, verify you have:

- [ ] Node.js v20+ installed (`node --version`)
- [ ] Bun v1.1+ installed (`bun --version`)
- [ ] VS Code installed and opening
- [ ] Git configured (`git config user.name`)
- [ ] TMDB API key saved (from setup.md)
- [ ] Completed [concepts.md](../02-learning-concepts/concepts.md)
- [ ] GitHub account created

If anything is missing, go back to [01-getting-started](../01-getting-started/) first!

---

## 🏗️ Project Setup

### Create Your Project

```bash
# Create a new Next.js project
bun create next-app@latest movie-explorer-learning

# During setup, answer like this:
# - TypeScript? → Yes
# - ESLint? → Yes
# - Tailwind CSS? → Yes
# - App Router? → Yes
# - Everything else → (defaults are fine)

# Enter the project
cd movie-explorer-learning

# Start development server
bun dev
```

### Verify It Works

1. Open browser to `http://localhost:3000`
2. You should see the Next.js welcome page
3. Press Ctrl+C to stop the server (you'll restart it later)

### Add Environment Variables

Create `.env.local` file in your project root:

```bash
NEXT_PUBLIC_TMDB_API_KEY=your_api_key_here
NEXT_PUBLIC_TMDB_BASE_URL=https://api.themoviedb.org/3
```

Replace `your_api_key_here` with your actual TMDB API key!

⚠️ **Important**: Add `.env.local` to `.gitignore` - it should already be there!

### Push to GitHub

```bash
# Initialize git (if not already done)
git init

# Add all files
git add .

# Initial commit
git commit -m "Initial: Create Next.js movie explorer project"

# Create repo on GitHub first, then:
git remote add origin https://github.com/YOUR_USERNAME/movie-explorer-learning.git
git branch -M main
git push -u origin main
```

---

# 📅 Day-by-Day Tasks

---

## ✨ Day 1: Project Setup & Search Component

**Time Estimate**: 3-4 hours
**Goals**: Create basic structure, build search component, understand API integration

### Learning Objectives

By end of Day 1, you should understand:
- How to structure a Next.js project
- How to create React components
- How to use useState hook
- How to handle form submissions
- How API routes work

### Tasks

#### Task 1.1: Create Project Structure

**What to do:**
1. Delete the default pages (clean up `app/page.js`)
2. Create these folders in `app/`:
   ```
   app/
   ├── components/
   │   └── SearchBar.jsx
   ├── api/
   │   └── search/
   │       └── route.js
   └── page.js
   ```

**Code for `app/page.js`:**

```javascript
'use client'

import SearchBar from './components/SearchBar'

export default function Home() {
  return (
    <div className="min-h-screen bg-gray-900 text-white p-4">
      <header className="mb-8">
        <h1 className="text-4xl font-bold mb-2">🎬 Movie Explorer</h1>
        <p className="text-gray-400">Search millions of movies</p>
      </header>

      <SearchBar />
    </div>
  )
}
```

**Acceptance Criteria:**
- [ ] Project structure is created
- [ ] page.js renders without errors
- [ ] Browser shows the header

---

#### Task 1.2: Create SearchBar Component

**What to do:**
Build a search component with an input and button.

**Code for `app/components/SearchBar.jsx`:**

```javascript
'use client'

import { useState } from 'react'

export default function SearchBar() {
  const [query, setQuery] = useState('')
  const [loading, setLoading] = useState(false)
  const [results, setResults] = useState([])
  const [error, setError] = useState('')

  const handleSearch = async (e) => {
    e.preventDefault()

    if (!query.trim()) {
      setError('Please enter a search term')
      return
    }

    setLoading(true)
    setError('')
    setResults([])

    try {
      const response = await fetch(`/api/search?q=${encodeURIComponent(query)}`)
      const data = await response.json()

      if (!response.ok) {
        setError(data.error || 'Search failed')
        return
      }

      setResults(data.results || [])
    } catch (err) {
      setError('Failed to fetch movies. Try again.')
      console.error(err)
    } finally {
      setLoading(false)
    }
  }

  return (
    <div>
      {/* Search Form */}
      <form onSubmit={handleSearch} className="mb-8 flex gap-2">
        <input
          type="text"
          value={query}
          onChange={(e) => setQuery(e.target.value)}
          placeholder="Search for a movie..."
          className="flex-1 px-4 py-2 rounded bg-gray-800 text-white border border-gray-700 focus:outline-none focus:border-blue-500"
        />
        <button
          type="submit"
          disabled={loading}
          className="px-6 py-2 bg-blue-600 rounded hover:bg-blue-700 disabled:opacity-50 disabled:cursor-not-allowed transition"
        >
          {loading ? 'Searching...' : 'Search'}
        </button>
      </form>

      {/* Error Message */}
      {error && (
        <div className="p-4 bg-red-900/20 border border-red-700 rounded mb-4 text-red-200">
          {error}
        </div>
      )}

      {/* Results Count */}
      {results.length > 0 && (
        <p className="text-gray-400 mb-4">Found {results.length} movies</p>
      )}

      {/* Results - For now, just show count */}
      {results.length > 0 && (
        <div className="text-gray-400">
          <p>Movies found! (Display coming tomorrow)</p>
        </div>
      )}
    </div>
  )
}
```

**Acceptance Criteria:**
- [ ] Component renders with input and button
- [ ] Input accepts text
- [ ] Button submits form without page reload
- [ ] Loading state shows "Searching..."

---

#### Task 1.3: Create API Route for Search

**What to do:**
Build a backend API route that calls TMDB API.

**Code for `app/api/search/route.js`:**

```javascript
export async function GET(request) {
  // Get the search query from URL
  const { searchParams } = new URL(request.url)
  const query = searchParams.get('q')

  // Validate input
  if (!query || query.trim() === '') {
    return Response.json(
      { error: 'Search query is required' },
      { status: 400 }
    )
  }

  try {
    // Call TMDB API
    const response = await fetch(
      `${process.env.NEXT_PUBLIC_TMDB_BASE_URL}/search/movie?query=${encodeURIComponent(query)}&api_key=${process.env.NEXT_PUBLIC_TMDB_API_KEY}`
    )

    // Check if API call was successful
    if (!response.ok) {
      throw new Error(`TMDB API error: ${response.status}`)
    }

    const data = await response.json()

    // Return only first 20 results
    return Response.json({
      results: data.results?.slice(0, 20) || []
    })
  } catch (error) {
    console.error('Search API error:', error)
    return Response.json(
      { error: 'Failed to fetch movies from TMDB' },
      { status: 500 }
    )
  }
}
```

**Acceptance Criteria:**
- [ ] API route exists at `/api/search`
- [ ] Accepts `q` query parameter
- [ ] Returns JSON with results array
- [ ] Handles errors gracefully

---

#### Task 1.4: Test the Search

**What to do:**
Test that search works end-to-end.

**Testing Steps:**
1. Start dev server: `bun dev`
2. Go to `http://localhost:3000`
3. Type "batman" in search box
4. Click Search
5. You should see "Found X movies"

**What Should Happen:**
- ✅ No errors in browser console (F12)
- ✅ Search returns results
- ✅ Loading state shows briefly
- ✅ Button is disabled while loading

**If It Doesn't Work:**
- Check browser console (F12) for errors
- Check server console (where you ran `bun dev`)
- Verify `.env.local` has your API key
- See [Troubleshooting Guide](../../reference/troubleshoot.md)

---

#### Task 1.5: Commit to GitHub

```bash
# Check what changed
git status

# Stage all changes
git add .

# Commit
git commit -m "Day 1: Build search component and API route"

# Push to GitHub
git push
```

---

### Day 1 Checklist

- [ ] Project structure created
- [ ] SearchBar component renders
- [ ] Input and button work
- [ ] API route exists and works
- [ ] Search returns results
- [ ] Error handling works
- [ ] Code committed to GitHub
- [ ] No console errors

---

## ✨ Day 2: Display Movie Results

**Time Estimate**: 2-3 hours
**Goals**: Show movie data beautifully, create MovieCard component

### Learning Objectives

By end of Day 2, you should understand:
- How to map over arrays in React
- How to pass props to components
- How to style with Tailwind CSS
- How to display images

### Tasks

#### Task 2.1: Create MovieCard Component

**What to do:**
Build a component to display individual movie info.

**Code for `app/components/MovieCard.jsx`:**

```javascript
'use client'

export default function MovieCard({ movie }) {
  const posterUrl = movie.poster_path
    ? `https://image.tmdb.org/t/p/w500${movie.poster_path}`
    : 'https://via.placeholder.com/300x450?text=No+Image'

  return (
    <div className="bg-gray-800 rounded-lg overflow-hidden hover:scale-105 transition transform">
      {/* Poster Image */}
      <div className="relative h-64 overflow-hidden bg-gray-700">
        <img
          src={posterUrl}
          alt={movie.title || 'Movie poster'}
          className="w-full h-full object-cover"
          onError={(e) => {
            e.target.src = 'https://via.placeholder.com/300x450?text=Error'
          }}
        />
      </div>

      {/* Movie Info */}
      <div className="p-4">
        <h3 className="font-bold text-lg mb-2 line-clamp-2">
          {movie.title}
        </h3>

        {/* Rating */}
        <div className="flex items-center gap-2 mb-2">
          <span className="text-yellow-400">⭐</span>
          <span className="text-sm text-gray-300">
            {movie.vote_average?.toFixed(1) || 'N/A'} / 10
          </span>
        </div>

        {/* Release Year */}
        <p className="text-xs text-gray-400 mb-3">
          {movie.release_date?.split('-')[0] || 'Unknown'}
        </p>

        {/* Description */}
        <p className="text-sm text-gray-300 line-clamp-3 mb-4">
          {movie.overview || 'No description available'}
        </p>

        {/* Favorite Button (for now, just show it) */}
        <button className="w-full py-2 bg-blue-600 hover:bg-blue-700 rounded transition text-sm font-medium">
          ❤️ Add to Favorites
        </button>
      </div>
    </div>
  )
}
```

**Acceptance Criteria:**
- [ ] Component displays movie title
- [ ] Shows movie poster image
- [ ] Displays rating
- [ ] Shows release year
- [ ] Has a favorites button

---

#### Task 2.2: Display Results Grid

**What to do:**
Update SearchBar to show all results in a grid.

**Update `app/components/SearchBar.jsx`:**

Add this import at the top:
```javascript
import MovieCard from './MovieCard'
```

Replace the "Results" section (after the error message) with:

```javascript
      {/* Results Grid */}
      {results.length > 0 && (
        <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
          {results.map((movie) => (
            <MovieCard key={movie.id} movie={movie} />
          ))}
        </div>
      )}

      {/* No Results Message */}
      {!loading && query && results.length === 0 && !error && (
        <div className="text-center py-12">
          <p className="text-gray-400 text-lg">No movies found for "{query}"</p>
          <p className="text-gray-500 text-sm">Try searching for something else</p>
        </div>
      )}
```

**Acceptance Criteria:**
- [ ] Results display in grid (4 columns on desktop)
- [ ] Grid responsive (1 col on mobile, 2 on tablet)
- [ ] Each movie shows poster, title, rating, year
- [ ] No results message shows when appropriate

---

#### Task 2.3: Test the Display

**Testing Steps:**
1. Start dev server: `bun dev`
2. Search for "batman"
3. You should see 20 movie cards in a grid
4. Resize browser window - grid should adapt
5. Check on phone (if available)

**What Should Happen:**
- ✅ Movies display in grid
- ✅ All movie info visible
- ✅ Images load (or show placeholder)
- ✅ Responsive on different screen sizes
- ✅ Hover effect on cards

---

#### Task 2.4: Add Image Optimization

**What to do:**
Use Next.js Image component for better performance.

**Update imports in `app/components/MovieCard.jsx`:**

```javascript
'use client'

import Image from 'next/image'
```

**Replace the image tag with:**

```javascript
        <img
          src={posterUrl}
          alt={movie.title || 'Movie poster'}
          className="w-full h-full object-cover"
          loading="lazy"
          onError={(e) => {
            e.target.src = 'https://via.placeholder.com/300x450?text=Error'
          }}
        />
```

**Acceptance Criteria:**
- [ ] Images load with `loading="lazy"`
- [ ] No console errors
- [ ] Page still responsive

---

#### Task 2.5: Commit to GitHub

```bash
git add .
git commit -m "Day 2: Display movie results in grid with cards"
git push
```

---

### Day 2 Checklist

- [ ] MovieCard component created
- [ ] Results display in grid
- [ ] Grid is responsive
- [ ] Movie info visible
- [ ] Images load or show placeholder
- [ ] No results message works
- [ ] Code committed to GitHub

---

## ✨ Day 3: Add Favorites Feature

**Time Estimate**: 2-3 hours
**Goals**: Save favorites to browser storage, display saved movies

### Learning Objectives

By end of Day 3, you should understand:
- How to use localStorage
- How to manage state across components
- How to persist data
- How useEffect works

### Tasks

#### Task 3.1: Create Favorites Hook

**What to do:**
Build a custom hook to manage favorites.

**Create `app/hooks/useFavorites.js`:**

```javascript
import { useState, useEffect } from 'react'

export function useFavorites() {
  const [favorites, setFavorites] = useState([])
  const [isLoading, setIsLoading] = useState(true)

  // Load favorites from localStorage on mount
  useEffect(() => {
    try {
      const stored = localStorage.getItem('movieFavorites')
      if (stored) {
        setFavorites(JSON.parse(stored))
      }
    } catch (error) {
      console.error('Failed to load favorites:', error)
    } finally {
      setIsLoading(false)
    }
  }, [])

  // Save to localStorage whenever favorites change
  useEffect(() => {
    if (!isLoading) {
      try {
        localStorage.setItem('movieFavorites', JSON.stringify(favorites))
      } catch (error) {
        console.error('Failed to save favorites:', error)
      }
    }
  }, [favorites, isLoading])

  const addFavorite = (movie) => {
    // Don't add duplicates
    if (!favorites.some((fav) => fav.id === movie.id)) {
      setFavorites([...favorites, movie])
    }
  }

  const removeFavorite = (movieId) => {
    setFavorites(favorites.filter((fav) => fav.id !== movieId))
  }

  const isFavorite = (movieId) => {
    return favorites.some((fav) => fav.id === movieId)
  }

  return { favorites, addFavorite, removeFavorite, isFavorite, isLoading }
}
```

**Acceptance Criteria:**
- [ ] Hook loads favorites on mount
- [ ] Hook saves to localStorage
- [ ] Can add favorites
- [ ] Can remove favorites
- [ ] Can check if movie is favorite

---

#### Task 3.2: Update MovieCard for Favorites

**What to do:**
Make favorites button functional.

**Update `app/components/MovieCard.jsx`:**

Add import:
```javascript
import { useFavorites } from '../hooks/useFavorites'
```

Update the component:
```javascript
'use client'

import { useFavorites } from '../hooks/useFavorites'

export default function MovieCard({ movie }) {
  const { isFavorite, addFavorite, removeFavorite } = useFavorites()
  const isLiked = isFavorite(movie.id)

  const posterUrl = movie.poster_path
    ? `https://image.tmdb.org/t/p/w500${movie.poster_path}`
    : 'https://via.placeholder.com/300x450?text=No+Image'

  const handleToggleFavorite = () => {
    if (isLiked) {
      removeFavorite(movie.id)
    } else {
      addFavorite(movie)
    }
  }

  return (
    <div className="bg-gray-800 rounded-lg overflow-hidden hover:scale-105 transition transform">
      {/* Poster Image */}
      <div className="relative h-64 overflow-hidden bg-gray-700">
        <img
          src={posterUrl}
          alt={movie.title || 'Movie poster'}
          className="w-full h-full object-cover"
          onError={(e) => {
            e.target.src = 'https://via.placeholder.com/300x450?text=Error'
          }}
        />
      </div>

      {/* Movie Info */}
      <div className="p-4">
        <h3 className="font-bold text-lg mb-2 line-clamp-2">
          {movie.title}
        </h3>

        <div className="flex items-center gap-2 mb-2">
          <span className="text-yellow-400">⭐</span>
          <span className="text-sm text-gray-300">
            {movie.vote_average?.toFixed(1) || 'N/A'} / 10
          </span>
        </div>

        <p className="text-xs text-gray-400 mb-3">
          {movie.release_date?.split('-')[0] || 'Unknown'}
        </p>

        <p className="text-sm text-gray-300 line-clamp-3 mb-4">
          {movie.overview || 'No description available'}
        </p>

        {/* Favorite Button - Now Functional */}
        <button
          onClick={handleToggleFavorite}
          className={`w-full py-2 rounded transition text-sm font-medium ${
            isLiked
              ? 'bg-red-600 hover:bg-red-700'
              : 'bg-blue-600 hover:bg-blue-700'
          }`}
        >
          {isLiked ? '❤️ Remove from Favorites' : '🤍 Add to Favorites'}
        </button>
      </div>
    </div>
  )
}
```

**Acceptance Criteria:**
- [ ] Button shows "Add to Favorites" initially
- [ ] Button changes to "Remove" when favorited
- [ ] Button color changes (red when favorited)
- [ ] Clicking adds/removes from favorites
- [ ] Favorites persist after page refresh

---

#### Task 3.3: Create Favorites Page

**What to do:**
Build a page to view all favorite movies.

**Create `app/components/FavoritesSection.jsx`:**

```javascript
'use client'

import { useFavorites } from '../hooks/useFavorites'
import MovieCard from './MovieCard'

export default function FavoritesSection() {
  const { favorites, isLoading } = useFavorites()

  if (isLoading) {
    return <div className="text-gray-400">Loading favorites...</div>
  }

  return (
    <section className="mb-12">
      <h2 className="text-2xl font-bold mb-4">
        ❤️ Your Favorites ({favorites.length})
      </h2>

      {favorites.length === 0 ? (
        <div className="text-center py-8 bg-gray-800/50 rounded">
          <p className="text-gray-400">No favorites yet</p>
          <p className="text-gray-500 text-sm">
            Search for movies and click the heart to add them here
          </p>
        </div>
      ) : (
        <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
          {favorites.map((movie) => (
            <MovieCard key={movie.id} movie={movie} />
          ))}
        </div>
      )}
    </section>
  )
}
```

**Update `app/page.js` to show favorites:**

```javascript
'use client'

import SearchBar from './components/SearchBar'
import FavoritesSection from './components/FavoritesSection'

export default function Home() {
  return (
    <div className="min-h-screen bg-gray-900 text-white p-4">
      <header className="mb-8">
        <h1 className="text-4xl font-bold mb-2">🎬 Movie Explorer</h1>
        <p className="text-gray-400">Search millions of movies</p>
      </header>

      {/* Show Favorites First */}
      <FavoritesSection />

      {/* Then Search Section */}
      <section>
        <h2 className="text-2xl font-bold mb-4">🔍 Search Movies</h2>
        <SearchBar />
      </section>
    </div>
  )
}
```

**Acceptance Criteria:**
- [ ] Favorites section shows at top
- [ ] Shows count of favorites
- [ ] Shows "No favorites" message when empty
- [ ] Displays favorite movies in grid
- [ ] Removing from favorites updates the section

---

#### Task 3.4: Test Favorites

**Testing Steps:**
1. Start dev server: `bun dev`
2. Search for "batman"
3. Click "Add to Favorites" on a movie
4. Check that favorites section updates
5. Refresh the page
6. Favorites should still be there!
7. Click "Remove" to delete

**What Should Happen:**
- ✅ Favorites section updates immediately
- ✅ Button text/color changes
- ✅ Favorites persist after refresh
- ✅ Can add and remove multiple movies

---

#### Task 3.5: Commit to GitHub

```bash
git add .
git commit -m "Day 3: Add favorites functionality with localStorage"
git push
```

---

### Day 3 Checklist

- [ ] useFavorites hook created
- [ ] Favorites save to localStorage
- [ ] Favorites load on page load
- [ ] MovieCard favorites button works
- [ ] FavoritesSection displays saved movies
- [ ] Favorites persist after refresh
- [ ] Can add and remove favorites
- [ ] Code committed to GitHub

---

## ✨ Day 4: Polish & Responsive Design

**Time Estimate**: 2-3 hours
**Goals**: Improve UI, fix mobile layout, add refinements

### Learning Objectives

By end of Day 4, you should understand:
- Responsive design with Tailwind
- Better UX with loading states
- Error handling
- Performance optimization

### Tasks

#### Task 4.1: Improve Mobile Responsiveness

**What to do:**
Ensure app looks great on all devices.

**Update `app/page.js`:**

```javascript
'use client'

import SearchBar from './components/SearchBar'
import FavoritesSection from './components/FavoritesSection'

export default function Home() {
  return (
    <div className="min-h-screen bg-gray-900 text-white">
      {/* Header */}
      <header className="sticky top-0 bg-gray-950/90 backdrop-blur border-b border-gray-800 p-4 z-10">
        <div className="max-w-6xl mx-auto">
          <h1 className="text-3xl md:text-4xl font-bold mb-1">🎬 Movie Explorer</h1>
          <p className="text-sm md:text-base text-gray-400">Search millions of movies</p>
        </div>
      </header>

      {/* Main Content */}
      <main className="max-w-6xl mx-auto p-4 md:p-6">
        {/* Show Favorites First */}
        <FavoritesSection />

        {/* Search Section */}
        <section className="mt-12">
          <h2 className="text-2xl md:text-3xl font-bold mb-4">🔍 Search Movies</h2>
          <SearchBar />
        </section>
      </main>

      {/* Footer */}
      <footer className="bg-gray-950 border-t border-gray-800 p-4 mt-12">
        <div className="max-w-6xl mx-auto text-center text-gray-500 text-sm">
          <p>Movie data from TMDB API | Built with Next.js & React</p>
        </div>
      </footer>
    </div>
  )
}
```

**Acceptance Criteria:**
- [ ] Header is sticky on scroll
- [ ] Layout works on mobile (375px)
- [ ] Layout works on tablet (768px)
- [ ] Layout works on desktop (1024px+)
- [ ] Text sizes adjust for each device
- [ ] Footer appears at bottom

---

#### Task 4.2: Add Loading Skeleton

**What to do:**
Show loading state while fetching movies.

**Create `app/components/MovieSkeleton.jsx`:**

```javascript
export default function MovieSkeleton() {
  return (
    <div className="bg-gray-800 rounded-lg overflow-hidden animate-pulse">
      {/* Poster Skeleton */}
      <div className="h-64 bg-gray-700"></div>

      {/* Info Skeleton */}
      <div className="p-4">
        <div className="h-6 bg-gray-700 rounded mb-3"></div>
        <div className="h-4 bg-gray-700 rounded w-2/3 mb-3"></div>
        <div className="h-4 bg-gray-700 rounded w-1/2 mb-4"></div>
        <div className="h-4 bg-gray-700 rounded mb-4"></div>
        <div className="h-10 bg-gray-700 rounded"></div>
      </div>
    </div>
  )
}
```

**Update `app/components/SearchBar.jsx` to show skeletons while loading:**

Add import:
```javascript
import MovieSkeleton from './MovieSkeleton'
```

Update the results section:
```javascript
      {/* Loading Skeletons */}
      {loading && (
        <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
          {Array.from({ length: 8 }).map((_, i) => (
            <MovieSkeleton key={i} />
          ))}
        </div>
      )}

      {/* Results Grid */}
      {!loading && results.length > 0 && (
        <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
          {results.map((movie) => (
            <MovieCard key={movie.id} movie={movie} />
          ))}
        </div>
      )}
```

**Acceptance Criteria:**
- [ ] Shows skeleton loaders while searching
- [ ] 8 skeletons display in grid
- [ ] Skeletons disappear when results load
- [ ] Smooth animation

---

#### Task 4.3: Add Error Retry

**What to do:**
Let users retry when search fails.

**Update the error section in `app/components/SearchBar.jsx`:**

```javascript
const [retryError, setRetryError] = useState(false)

const handleRetry = () => {
  setRetryError(false)
  handleSearch(new Event('submit'))
}

// Update error display:
      {/* Error Message with Retry */}
      {error && (
        <div className="p-4 bg-red-900/20 border border-red-700 rounded mb-4 flex items-center justify-between">
          <span className="text-red-200">{error}</span>
          <button
            onClick={handleRetry}
            className="px-3 py-1 bg-red-700 hover:bg-red-600 rounded text-sm transition"
          >
            Retry
          </button>
        </div>
      )}
```

**Acceptance Criteria:**
- [ ] Error shows with message
- [ ] Retry button appears
- [ ] Retry button triggers search again

---

#### Task 4.4: Optimize Performance

**What to do:**
Make the app faster.

**Update `app/components/SearchBar.jsx`:**

Add debounce for better UX:
```javascript
const [query, setQuery] = useState('')
const [searchTimeout, setSearchTimeout] = useState(null)

const handleInputChange = (e) => {
  setQuery(e.target.value)

  // Auto-search after 500ms of typing (optional)
  clearTimeout(searchTimeout)
  // You could add auto-search here if desired
}
```

**Acceptance Criteria:**
- [ ] App loads quickly
- [ ] No unnecessary re-renders
- [ ] Search responds quickly
- [ ] Scrolling is smooth

---

#### Task 4.5: Test Everything

**Testing Steps:**
1. Open app on desktop
2. Search for movie - should show skeletons then results
3. Open DevTools (F12) - simulate mobile
4. Test on different screen sizes
5. Try error (disconnect internet, then search)
6. Click retry button
7. Add some favorites
8. Refresh page - favorites should be there

**What Should Happen:**
- ✅ Looks good on all screen sizes
- ✅ Loading states work
- ✅ Error handling works
- ✅ Favorites persist
- ✅ No console errors

---

#### Task 4.6: Commit to GitHub

```bash
git add .
git commit -m "Day 4: Polish UI, improve responsiveness, add loading states"
git push
```

---

### Day 4 Checklist

- [ ] Mobile responsiveness verified
- [ ] Sticky header works
- [ ] Loading skeletons display
- [ ] Error retry button works
- [ ] Performance optimized
- [ ] All screen sizes tested
- [ ] Code committed to GitHub

---

## ✨ Day 5: Final Polish & Deployment Prep

**Time Estimate**: 2-3 hours
**Goals**: Clean code, final testing, prepare for deployment

### Learning Objectives

By end of Day 5, you should understand:
- Code organization and cleanup
- Testing your app thoroughly
- Preparing for production
- Deployment basics

### Tasks

#### Task 5.1: Code Cleanup

**What to do:**
Clean up code, remove console.logs, fix any issues.

**Checklist:**
- [ ] Remove all `console.log()` statements (keep only errors)
- [ ] Check for unused imports
- [ ] Add comments to complex code
- [ ] Fix any ESLint warnings

**Find and fix issues:**
```bash
# Check for issues
npm run lint
# or
bun run lint
```

**Fix simple issues automatically:**
```bash
npm run lint -- --fix
# or
bun run lint -- --fix
```

---

#### Task 5.2: Accessibility Improvements

**What to do:**
Make app accessible to everyone.

**Update `app/components/MovieCard.jsx`:**
- Add `alt` text (already have it)
- Add `aria-label` to buttons

```javascript
<button
  onClick={handleToggleFavorite}
  aria-label={isLiked ? 'Remove from favorites' : 'Add to favorites'}
  className={`w-full py-2 rounded transition text-sm font-medium ${
    isLiked
      ? 'bg-red-600 hover:bg-red-700'
      : 'bg-blue-600 hover:bg-blue-700'
  }`}
>
  {isLiked ? '❤️ Remove from Favorites' : '🤍 Add to Favorites'}
</button>
```

**Update `app/components/SearchBar.jsx`:**

```javascript
<input
  type="text"
  value={query}
  onChange={(e) => setQuery(e.target.value)}
  placeholder="Search for a movie..."
  aria-label="Search for movies"
  className="flex-1 px-4 py-2 rounded bg-gray-800 text-white border border-gray-700 focus:outline-none focus:border-blue-500"
/>
```

**Acceptance Criteria:**
- [ ] Images have alt text
- [ ] Buttons have aria labels
- [ ] Keyboard navigation works (Tab key)
- [ ] Color contrast is good
- [ ] Focus indicators visible

---

#### Task 5.3: Final Testing

**What to do:**
Test everything thoroughly.

**Desktop Testing:**
```
[ ] Search functionality works
[ ] Results display correctly
[ ] Favorites add/remove works
[ ] Favorites persist on refresh
[ ] Error handling works
[ ] Loading states show
```

**Mobile Testing:**
```
[ ] Responsive layout works
[ ] Touch interactions work
[ ] Scrolling is smooth
[ ] Images load quickly
[ ] No horizontal scrollbar
```

**Browser Testing:**
```
[ ] Chrome: works
[ ] Firefox: works
[ ] Safari: works
[ ] Edge: works
```

**Network Testing:**
```
[ ] Fast network: loads quickly
[ ] Slow network: shows loading state
[ ] No network: shows error
[ ] Error recovery works
```

---

#### Task 5.4: Update README

**What to do:**
Create a README for your project.

**Create `README.md` in project root:**

```markdown
# 🎬 Movie Explorer

A movie search app built with Next.js and React.

## Features

- 🔍 Search movies by title
- 📸 View movie posters and details
- ❤️ Save your favorite movies
- 📱 Works on all devices
- 🚀 Lightning fast performance

## Tech Stack

- **Frontend**: React, Next.js, Tailwind CSS
- **Data**: TMDB API
- **Storage**: Browser localStorage
- **Deployment**: Vercel

## Getting Started

### Prerequisites
- Node.js v20+
- Bun v1.1+
- TMDB API key (get free at https://www.themoviedb.org/settings/api)

### Installation

1. Clone the repo
```bash
git clone https://github.com/YOUR_USERNAME/movie-explorer-learning.git
cd movie-explorer-learning
```

2. Install dependencies
```bash
bun install
```

3. Create `.env.local`
```bash
NEXT_PUBLIC_TMDB_API_KEY=your_api_key_here
NEXT_PUBLIC_TMDB_BASE_URL=https://api.themoviedb.org/3
```

4. Start dev server
```bash
bun dev
```

5. Open http://localhost:3000

## Project Structure

```
app/
├── components/       # React components
├── api/             # API routes
├── hooks/           # Custom hooks
└── page.js          # Home page
```

## What I Learned

- React components and hooks
- API integration
- localStorage for persistence
- Responsive design
- Error handling
- Git and GitHub

## Future Improvements

- [ ] Add movie details page
- [ ] User authentication
- [ ] Share favorites with friends
- [ ] Movie recommendations
- [ ] Advanced filtering

## Live Demo

Visit: https://your-deployment.vercel.app

## Author

YOUR_NAME

## License

MIT
```

---

#### Task 5.5: Final Commit

```bash
git add .
git commit -m "Day 5: Final polish, accessibility improvements, and documentation"
git push
```

---

### Day 5 Checklist

- [ ] Code cleaned up
- [ ] ESLint warnings fixed
- [ ] Accessibility improved
- [ ] Desktop testing passed
- [ ] Mobile testing passed
- [ ] Browser testing passed
- [ ] Network testing passed
- [ ] README created and complete
- [ ] Final commit pushed

---

## ✅ Project Completion Checklist

Before deploying, verify ALL of these:

### Core Features
- [ ] Search works
- [ ] Results display correctly
- [ ] Favorites add/remove works
- [ ] Favorites persist
- [ ] Error handling works
- [ ] Loading states show

### Design & UX
- [ ] Mobile responsive
- [ ] Looks good on desktop
- [ ] Smooth animations
- [ ] Good loading states
- [ ] Error messages clear
- [ ] Accessibility good

### Code Quality
- [ ] No console.logs
- [ ] No ESLint errors
- [ ] Code commented where needed
- [ ] No unused imports
- [ ] Variable names are clear

### Testing
- [ ] Tested on Chrome
- [ ] Tested on Firefox
- [ ] Tested on Safari
- [ ] Tested on mobile
- [ ] Tested with slow network
- [ ] Tested error cases

### Documentation
- [ ] README.md complete
- [ ] Code has comments
- [ ] Git commit messages clear
- [ ] All on GitHub

---

## 🚀 Ready to Deploy?

Once you've completed all 5 days and verified everything works:

Go to [04 - Deployment](../../04-deployment/) and follow the guide to deploy to Vercel!

Your app will be live at a URL like: `https://movie-explorer-xxx.vercel.app`

---

## 📚 Helpful References

- **React Hooks**: https://react.dev/reference/react/hooks
- **Next.js Guide**: https://nextjs.org/docs
- **Tailwind CSS**: https://tailwindcss.com/docs
- **TMDB API**: https://www.themoviedb.org/settings/api
- **JavaScript**: https://developer.mozilla.org/en-US/docs/Web/JavaScript

---

## 🆘 Getting Help

If you get stuck:

1. Check [../../reference/troubleshoot.md](../../reference/troubleshoot.md)
2. Search your error on Google/Stack Overflow
3. Check the relevant documentation
4. Ask in communities (see resources.md)

---

## 🎉 You've Done It!

By completing this PRD, you've:
- ✅ Built a real web application
- ✅ Learned React and Next.js
- ✅ Integrated with a public API
- ✅ Deployed to the internet
- ✅ Created a portfolio project

**Congratulations! You're now a web developer! 🎊**

---

*Last Updated: October 31, 2025*
*Estimated Time: 1-2 weeks (5-10 hours per week)*
*Difficulty: Beginner-Friendly*
