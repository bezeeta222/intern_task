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
# Create a new Next.js project with TypeScript
bun create next-app@latest movie-explorer-learning

# During setup, answer like this:
# - TypeScript? → Yes ✅
# - ESLint? → Yes
# - Tailwind CSS? → Yes ✅
# - App Router? → Yes ✅
# - Import alias? → Yes (default @/*)
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

### Install shadcn/ui

shadcn/ui is a collection of beautiful, accessible components built on top of Tailwind CSS.

```bash
# Initialize shadcn/ui
bun x shadcn-ui@latest init

# Answer the prompts:
# - Style: Default
# - Base color: Slate
# - CSS variables: Yes

# Install the components we'll use
bun x shadcn-ui@latest add button
bun x shadcn-ui@latest add input
bun x shadcn-ui@latest add card
bun x shadcn-ui@latest add badge
bun x shadcn-ui@latest add skeleton
bun x shadcn-ui@latest add alert
```

**What this does**:
- Creates `components.json` configuration
- Creates `lib/utils.ts` for utility functions
- Creates `components/ui/` folder with all installed components
- Now you can import: `import { Button } from '@/components/ui/button'`

### Add Environment Variables

Create `.env.local` file in your project root:

```bash
NEXT_PUBLIC_TMDB_API_KEY=your_api_key_here
NEXT_PUBLIC_TMDB_BASE_URL=https://api.themoviedb.org/3
```

Replace `your_api_key_here` with your actual TMDB API key!

⚠️ **Important**: Add `.env.local` to `.gitignore` - it should already be there!

### Verify Project Structure

Your project should now have:

```
movie-explorer-learning/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   └── api/
├── components/
│   ├── ui/              ← shadcn/ui components
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   ├── card.tsx
│   │   └── ...
│   └── (your components)
├── lib/
│   └── utils.ts         ← shadcn utilities
├── tsconfig.json        ← TypeScript config
├── tailwind.config.ts   ← Tailwind config
├── components.json      ← shadcn config
└── .env.local
```

### Push to GitHub

```bash
# Initialize git (if not already done)
git init

# Add all files
git add .

# Initial commit
git commit -m "Initial: Create Next.js movie explorer with TypeScript and shadcn/ui"

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

#### Task 1.1: Create Project Structure with TypeScript

**What to do:**
1. Delete the default pages (clean up `app/page.tsx`)
2. Create folder structure:
   ```
   app/
   ├── components/
   │   └── SearchBar.tsx
   ├── api/
   │   └── search/
   │       └── route.ts
   ├── layout.tsx
   └── page.tsx
   ```

**Code for `app/page.tsx`:**

```typescript
'use client'

import SearchBar from './components/SearchBar'

export default function Home() {
  return (
    <div className="min-h-screen bg-white p-4">
      <header className="mb-8 border-b pb-4">
        <h1 className="text-4xl font-bold mb-2">🎬 Movie Explorer</h1>
        <p className="text-gray-600">Search millions of movies with TypeScript & shadcn/ui</p>
      </header>

      <main>
        <SearchBar />
      </main>
    </div>
  )
}
```

**Acceptance Criteria:**
- [ ] Project structure created
- [ ] page.tsx renders without errors
- [ ] Browser shows header with styling
- [ ] No TypeScript errors in terminal

---

#### Task 1.1b: Create TypeScript Types File

Create `lib/types.ts` to define all our data types:

```typescript
// lib/types.ts

export interface Movie {
  id: number
  title: string
  poster_path: string | null
  vote_average: number
  release_date: string
  overview: string
}

export interface SearchResponse {
  results: Movie[]
  total_results: number
  total_pages: number
}

export interface SearchState {
  query: string
  loading: boolean
  results: Movie[]
  error: string
}
```

**Acceptance Criteria:**
- [ ] `lib/types.ts` file created
- [ ] All interfaces defined
- [ ] No TypeScript errors

---

#### Task 1.2: Create SearchBar Component with shadcn/ui

**What to do:**
Build a search component using shadcn/ui Button and Input components with full TypeScript typing.

**Code for `app/components/SearchBar.tsx`:**

```typescript
'use client'

import { useState, FormEvent } from 'react'
import { Input } from '@/components/ui/input'
import { Button } from '@/components/ui/button'
import { Alert, AlertDescription } from '@/components/ui/alert'
import { Movie, SearchState } from '@/lib/types'

interface SearchBarProps {
  onSearchResults?: (results: Movie[]) => void
}

export default function SearchBar({ onSearchResults }: SearchBarProps) {
  const [state, setState] = useState<SearchState>({
    query: '',
    loading: false,
    results: [],
    error: ''
  })

  const handleSearch = async (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault()

    if (!state.query.trim()) {
      setState(prev => ({ ...prev, error: 'Please enter a search term' }))
      return
    }

    setState(prev => ({
      ...prev,
      loading: true,
      error: '',
      results: []
    }))

    try {
      const response = await fetch(
        `/api/search?q=${encodeURIComponent(state.query)}`
      )
      const data = await response.json()

      if (!response.ok) {
        setState(prev => ({
          ...prev,
          error: data.error || 'Search failed',
          loading: false
        }))
        return
      }

      setState(prev => ({
        ...prev,
        results: data.results || [],
        loading: false
      }))

      onSearchResults?.(data.results || [])
    } catch (error) {
      setState(prev => ({
        ...prev,
        error: 'Failed to fetch movies. Try again.',
        loading: false
      }))
    }
  }

  return (
    <section className="w-full max-w-2xl mx-auto">
      {/* Search Form */}
      <form onSubmit={handleSearch} className="mb-6 flex gap-2">
        <Input
          type="text"
          value={state.query}
          onChange={(e) => setState(prev => ({ ...prev, query: e.target.value }))}
          placeholder="Search for a movie..."
          className="flex-1"
        />
        <Button type="submit" disabled={state.loading}>
          {state.loading ? 'Searching...' : 'Search'}
        </Button>
      </form>

      {/* Error Message */}
      {state.error && (
        <Alert variant="destructive" className="mb-4">
          <AlertDescription>{state.error}</AlertDescription>
        </Alert>
      )}

      {/* Results Count */}
      {state.results.length > 0 && (
        <p className="text-sm text-gray-600 mb-4">
          Found {state.results.length} movies
        </p>
      )}

      {/* Results - For now, just show count */}
      {state.results.length > 0 && (
        <div className="text-gray-600">
          <p>Movies found! (Display coming Day 2)</p>
        </div>
      )}
    </section>
  )
}
```

**Key TypeScript & shadcn/ui Features**:
- ✅ Proper `FormEvent<HTMLFormElement>` type for form handling
- ✅ `SearchBarProps` interface for component props
- ✅ Uses `SearchState` type from `lib/types.ts`
- ✅ shadcn/ui `<Input />` component
- ✅ shadcn/ui `<Button />` component with disabled state
- ✅ shadcn/ui `<Alert />` for error messages
- ✅ Full state management with proper types

**Acceptance Criteria:**
- [ ] Component renders with shadcn Input and Button
- [ ] Input accepts text
- [ ] Button submits form without page reload
- [ ] Loading state shows "Searching..."
- [ ] Error messages display with Alert component
- [ ] No TypeScript errors in terminal

---

#### Task 1.3: Create API Route for Search with TypeScript

**What to do:**
Build a backend API route that calls TMDB API with proper TypeScript types and error handling.

**Code for `app/api/search/route.ts`:**

```typescript
import { NextRequest } from 'next/server'
import { SearchResponse, Movie } from '@/lib/types'

// Type for TMDB API response
interface TMDBResponse {
  results: Array<{
    id: number
    title: string
    poster_path: string | null
    vote_average: number
    release_date: string
    overview: string
  }>
}

export async function GET(request: NextRequest): Promise<Response> {
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

  // Get API credentials from environment
  const baseUrl = process.env.NEXT_PUBLIC_TMDB_BASE_URL
  const apiKey = process.env.NEXT_PUBLIC_TMDB_API_KEY

  if (!baseUrl || !apiKey) {
    return Response.json(
      { error: 'API configuration missing' },
      { status: 500 }
    )
  }

  try {
    // Call TMDB API with typed response
    const response = await fetch(
      `${baseUrl}/search/movie?query=${encodeURIComponent(query)}&api_key=${apiKey}`,
      { next: { revalidate: 60 } } // Cache for 60 seconds
    )

    // Check if API call was successful
    if (!response.ok) {
      throw new Error(`TMDB API error: ${response.status}`)
    }

    const data = await response.json() as TMDBResponse

    // Return typed response with first 20 results
    const results: Movie[] = (data.results?.slice(0, 20) || []).map(movie => ({
      id: movie.id,
      title: movie.title,
      poster_path: movie.poster_path,
      vote_average: movie.vote_average,
      release_date: movie.release_date,
      overview: movie.overview
    }))

    const searchResponse: SearchResponse = {
      results,
      total_results: data.results.length,
      total_pages: 1
    }

    return Response.json(searchResponse)
  } catch (error) {
    const errorMessage = error instanceof Error ? error.message : 'Unknown error'
    console.error('Search API error:', errorMessage)

    return Response.json(
      { error: 'Failed to fetch movies from TMDB' },
      { status: 500 }
    )
  }
}
```

**Key TypeScript Features**:
- ✅ `NextRequest` type for proper request handling
- ✅ `TMDBResponse` interface for API response structure
- ✅ Typed return type: `Promise<Response>`
- ✅ Type-safe array mapping
- ✅ Proper error type checking with `instanceof`
- ✅ Returns `SearchResponse` type defined in `lib/types.ts`

**Acceptance Criteria:**
- [ ] API route exists at `/api/search` with `.ts` extension
- [ ] Accepts `q` query parameter
- [ ] Returns JSON with `SearchResponse` type
- [ ] Handles errors gracefully
- [ ] No TypeScript errors in terminal
- [ ] Uses environment variables safely

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
**Goals**: Show movie data beautifully using shadcn/ui Card component

### Learning Objectives

By end of Day 2, you should understand:
- How to map over arrays in React
- How to pass typed props to components with TypeScript
- How to use shadcn/ui components
- How to display images with Next.js Image component

### Prerequisites

Make sure you have shadcn/ui Card, Badge components installed:

```bash
bun x shadcn-ui@latest add card
bun x shadcn-ui@latest add badge
```

### Tasks

#### Task 2.1: Create MovieCard Component

**What to do:**
Build a component to display individual movie info using shadcn/ui Card.

**Create `app/components/MovieCard.tsx`:**

```typescript
'use client'

import Image from 'next/image'
import { Card, CardContent, CardHeader } from '@/components/ui/card'
import { Badge } from '@/components/ui/badge'
import { Button } from '@/components/ui/button'
import { Movie } from '@/lib/types'

interface MovieCardProps {
  movie: Movie
  onAddFavorite?: (movie: Movie) => void
  isFavorite?: boolean
}

export default function MovieCard({
  movie,
  onAddFavorite,
  isFavorite = false
}: MovieCardProps) {
  const posterUrl = movie.poster_path
    ? `https://image.tmdb.org/t/p/w500${movie.poster_path}`
    : 'https://via.placeholder.com/300x450?text=No+Image'

  const year = movie.release_date?.split('-')[0] || 'Unknown'
  const rating = movie.vote_average?.toFixed(1) || 'N/A'

  return (
    <Card className="overflow-hidden hover:shadow-lg hover:scale-105 transition-all duration-200">
      {/* Poster Image */}
      <CardHeader className="p-0">
        <div className="relative h-64 w-full bg-gray-200">
          <Image
            src={posterUrl}
            alt={movie.title || 'Movie poster'}
            fill
            className="object-cover"
            onError={(result) => {
              // Next.js Image handles errors gracefully
              result.target.src = 'https://via.placeholder.com/300x450?text=No+Image'
            }}
          />
        </div>
      </CardHeader>

      {/* Movie Info */}
      <CardContent className="p-4">
        {/* Title */}
        <h3 className="font-bold text-lg mb-2 line-clamp-2">
          {movie.title}
        </h3>

        {/* Rating Badge */}
        <div className="flex items-center gap-2 mb-3">
          <span className="text-lg">⭐</span>
          <Badge variant="secondary">
            {rating} / 10
          </Badge>
          <Badge variant="outline">
            {year}
          </Badge>
        </div>

        {/* Overview */}
        <p className="text-sm text-gray-600 line-clamp-3 mb-4">
          {movie.overview || 'No description available'}
        </p>

        {/* Favorite Button */}
        <Button
          onClick={() => onAddFavorite?.(movie)}
          variant={isFavorite ? 'destructive' : 'default'}
          className="w-full"
          disabled={!onAddFavorite}
        >
          {isFavorite ? '❤️ Remove from Favorites' : '🤍 Add to Favorites'}
        </Button>
      </CardContent>
    </Card>
  )
}
```

**What This Code Does:**
- Uses shadcn/ui `Card` component for clean styling
- Uses `Badge` for rating and year (professional look)
- Uses Next.js `Image` component for optimized image loading
- TypeScript `MovieCardProps` interface ensures type safety
- Optional `onAddFavorite` callback for favorites (we'll implement Day 3)

**Acceptance Criteria:**
- [ ] Component displays movie title
- [ ] Shows movie poster image using Next.js Image
- [ ] Displays rating in Badge
- [ ] Shows release year in Badge
- [ ] Has a favorites button
- [ ] No TypeScript errors

---

#### Task 2.2: Install shadcn/ui Components

**What to do:**
Make sure MovieCard dependencies are installed.

```bash
# Install components if not already done
bun x shadcn-ui@latest add card
bun x shadcn-ui@latest add badge
bun x shadcn-ui@latest add button

# Verify installation
ls -la app/components/ui/
```

You should see: `card.tsx`, `badge.tsx`, `button.tsx`

---

#### Task 2.3: Update SearchBar to Display Results

**What to do:**
Update SearchBar component to show movie results in a grid.

**Update `app/components/SearchBar.tsx`:**

Add this import at the top:
```typescript
import MovieCard from './MovieCard'
```

Replace your existing return statement's results section with:

```typescript
      {/* Results Grid */}
      {state.results.length > 0 && (
        <div className="mt-8">
          <h2 className="text-2xl font-bold mb-4">
            Found {state.results.length} movies
          </h2>
          <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
            {state.results.map((movie) => (
              <MovieCard
                key={movie.id}
                movie={movie}
                isFavorite={false}
              />
            ))}
          </div>
        </div>
      )}

      {/* No Results Message */}
      {!state.loading && state.query && state.results.length === 0 && !state.error && (
        <div className="text-center py-12 mt-8">
          <p className="text-lg font-medium">No movies found for "{state.query}"</p>
          <p className="text-gray-500 mt-2">Try searching for something else</p>
        </div>
      )}
```

**Acceptance Criteria:**
- [ ] Results display in responsive grid
- [ ] Grid is 4 columns on desktop, 1 on mobile
- [ ] Each movie card shows all info
- [ ] No results message appears when appropriate
- [ ] No TypeScript errors

---

#### Task 2.4: Test the Display

**Testing Steps:**
1. Make sure dev server running: `bun dev`
2. Open browser to `http://localhost:3000`
3. Search for "batman"
4. You should see movie cards in a grid
5. Resize browser window - grid adapts automatically
6. Check: hover effect on cards, images load

**What Should Happen:**
- ✅ Movies display in responsive grid
- ✅ All movie info visible (title, rating, year, description)
- ✅ Images load with Next.js Image (optimized)
- ✅ Cards have nice hover effect
- ✅ Grid adapts to screen size
- ✅ No console errors

**If Errors Occur:**
- Check browser console (F12)
- Check terminal where you ran `bun dev`
- Verify `lib/types.ts` exists
- Verify shadcn components installed: `ls app/components/ui/`

---

#### Task 2.5: Commit to GitHub

```bash
# Check what changed
git status

# Stage all changes
git add .

# Commit with clear message
git commit -m "Day 2: Display movie results in responsive grid with shadcn/ui Card"

# Push to GitHub
git push
```

---

### Day 2 Checklist

- [ ] MovieCard component created (MovieCard.tsx)
- [ ] MovieCard uses shadcn/ui Card and Badge
- [ ] MovieCard uses TypeScript with MovieCardProps interface
- [ ] SearchBar imports and displays MovieCard
- [ ] Results display in responsive grid
- [ ] Grid shows 1 col on mobile, 4 cols on desktop
- [ ] Movie info visible (title, rating, year, description)
- [ ] Images load correctly
- [ ] No results message works
- [ ] No TypeScript errors
- [ ] All code committed to GitHub

---

## ✨ Day 3: Add Favorites Feature

**Time Estimate**: 2-3 hours
**Goals**: Save favorites to browser storage with TypeScript, display saved movies

### Learning Objectives

By end of Day 3, you should understand:
- How to use localStorage safely
- How to create custom hooks with TypeScript
- How to manage state across components with typing
- How useEffect works with dependencies
- How to prevent infinite loops

### Tasks

#### Task 3.1: Create Favorites Hook with TypeScript

**What to do:**
Build a custom hook to manage favorites with full type safety.

**Create `app/hooks/useFavorites.ts`:**

```typescript
'use client'

import { useState, useEffect } from 'react'
import { Movie } from '@/lib/types'

interface UseFavoritesReturn {
  favorites: Movie[]
  addFavorite: (movie: Movie) => void
  removeFavorite: (movieId: number) => void
  isFavorite: (movieId: number) => boolean
  isLoading: boolean
}

const STORAGE_KEY = 'movieFavorites'

export function useFavorites(): UseFavoritesReturn {
  const [favorites, setFavorites] = useState<Movie[]>([])
  const [isLoading, setIsLoading] = useState(true)

  // Load favorites from localStorage on component mount
  useEffect(() => {
    try {
      // Only run in browser (localStorage not available on server)
      if (typeof window !== 'undefined') {
        const stored = localStorage.getItem(STORAGE_KEY)
        if (stored) {
          const parsed = JSON.parse(stored) as Movie[]
          setFavorites(parsed)
        }
      }
    } catch (error) {
      console.error('Failed to load favorites from localStorage:', error)
      // Continue without favorites if loading fails
    } finally {
      setIsLoading(false)
    }
  }, [])

  // Save to localStorage whenever favorites change
  useEffect(() => {
    // Don't save while still loading initial data
    if (!isLoading && typeof window !== 'undefined') {
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(favorites))
      } catch (error) {
        console.error('Failed to save favorites to localStorage:', error)
      }
    }
  }, [favorites, isLoading])

  const addFavorite = (movie: Movie): void => {
    // Prevent duplicates by checking if movie already exists
    if (!favorites.some((fav) => fav.id === movie.id)) {
      setFavorites([...favorites, movie])
    }
  }

  const removeFavorite = (movieId: number): void => {
    setFavorites(favorites.filter((fav) => fav.id !== movieId))
  }

  const isFavorite = (movieId: number): boolean => {
    return favorites.some((fav) => fav.id === movieId)
  }

  return {
    favorites,
    addFavorite,
    removeFavorite,
    isFavorite,
    isLoading
  }
}
```

**What This Code Does:**
- `UseFavoritesReturn` interface ensures hook returns consistent types
- `Movie` type imported from lib/types ensures type safety
- Checks `typeof window !== 'undefined'` (localStorage only works in browser)
- Loads favorites from localStorage on mount
- Saves to localStorage whenever favorites change
- Prevents duplicate favorites
- Returns loading state for UI handling

**Acceptance Criteria:**
- [ ] Hook file created: `app/hooks/useFavorites.ts` (not .js)
- [ ] Uses TypeScript interfaces
- [ ] Loads favorites on mount
- [ ] Saves to localStorage
- [ ] Can add favorites (no duplicates)
- [ ] Can remove favorites by ID
- [ ] Can check if movie is favorite
- [ ] No TypeScript errors

---

#### Task 3.2: Update MovieCard to Use Favorites Hook

**What to do:**
Integrate the favorites hook into MovieCard to make button functional.

**Update `app/components/MovieCard.tsx`:**

Replace the function with this updated version:

```typescript
'use client'

import Image from 'next/image'
import { Card, CardContent, CardHeader } from '@/components/ui/card'
import { Badge } from '@/components/ui/badge'
import { Button } from '@/components/ui/button'
import { Movie } from '@/lib/types'
import { useFavorites } from '@/app/hooks/useFavorites'

interface MovieCardProps {
  movie: Movie
}

export default function MovieCard({ movie }: MovieCardProps) {
  const { isFavorite, addFavorite, removeFavorite } = useFavorites()
  const isLiked = isFavorite(movie.id)

  const posterUrl = movie.poster_path
    ? `https://image.tmdb.org/t/p/w500${movie.poster_path}`
    : 'https://via.placeholder.com/300x450?text=No+Image'

  const year = movie.release_date?.split('-')[0] || 'Unknown'
  const rating = movie.vote_average?.toFixed(1) || 'N/A'

  const handleToggleFavorite = (): void => {
    if (isLiked) {
      removeFavorite(movie.id)
    } else {
      addFavorite(movie)
    }
  }

  return (
    <Card className="overflow-hidden hover:shadow-lg hover:scale-105 transition-all duration-200">
      {/* Poster Image */}
      <CardHeader className="p-0">
        <div className="relative h-64 w-full bg-gray-200">
          <Image
            src={posterUrl}
            alt={movie.title || 'Movie poster'}
            fill
            className="object-cover"
          />
        </div>
      </CardHeader>

      {/* Movie Info */}
      <CardContent className="p-4">
        {/* Title */}
        <h3 className="font-bold text-lg mb-2 line-clamp-2">
          {movie.title}
        </h3>

        {/* Rating and Year */}
        <div className="flex items-center gap-2 mb-3">
          <span className="text-lg">⭐</span>
          <Badge variant="secondary">
            {rating} / 10
          </Badge>
          <Badge variant="outline">
            {year}
          </Badge>
        </div>

        {/* Overview */}
        <p className="text-sm text-gray-600 line-clamp-3 mb-4">
          {movie.overview || 'No description available'}
        </p>

        {/* Favorite Button - Now Functional */}
        <Button
          onClick={handleToggleFavorite}
          variant={isLiked ? 'destructive' : 'default'}
          className="w-full"
        >
          {isLiked ? '❤️ Remove from Favorites' : '🤍 Add to Favorites'}
        </Button>
      </CardContent>
    </Card>
  )
}
```

**What Changed:**
- Removed `onAddFavorite` prop - now uses hook directly
- Uses `useFavorites()` hook for state management
- `handleToggleFavorite` now has `: void` return type
- Proper TypeScript button event handling
- Uses shadcn Button variants for styling

**Acceptance Criteria:**
- [ ] Button shows correct text (Add/Remove)
- [ ] Button changes color based on favorite state
- [ ] Clicking toggles favorite
- [ ] Favorites persist after page refresh
- [ ] No TypeScript errors

---

#### Task 3.3: Create FavoritesSection Component

**What to do:**
Build a section to display all favorite movies at top of page.

**Create `app/components/FavoritesSection.tsx`:**

```typescript
'use client'

import { Card, CardContent } from '@/components/ui/card'
import { useFavorites } from '@/app/hooks/useFavorites'
import MovieCard from './MovieCard'

export default function FavoritesSection() {
  const { favorites, isLoading } = useFavorites()

  // Show loading state
  if (isLoading) {
    return (
      <div className="mb-12">
        <h2 className="text-2xl font-bold mb-4">❤️ Your Favorites</h2>
        <p className="text-gray-500">Loading...</p>
      </div>
    )
  }

  // Show empty state
  if (favorites.length === 0) {
    return (
      <section className="mb-12">
        <h2 className="text-2xl font-bold mb-4">
          ❤️ Your Favorites ({favorites.length})
        </h2>
        <Card className="border-dashed">
          <CardContent className="p-8 text-center">
            <p className="text-lg font-medium mb-2">No favorites yet</p>
            <p className="text-gray-500">
              Search for movies and click the heart icon to save them here
            </p>
          </CardContent>
        </Card>
      </section>
    )
  }

  // Show favorites grid
  return (
    <section className="mb-12">
      <h2 className="text-2xl font-bold mb-4">
        ❤️ Your Favorites ({favorites.length})
      </h2>
      <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
        {favorites.map((movie) => (
          <MovieCard key={movie.id} movie={movie} />
        ))}
      </div>
    </section>
  )
}
```

**What This Code Does:**
- Displays loading state while favorites load
- Shows empty state with helpful message
- Displays favorite count in heading
- Maps over favorites array showing MovieCard for each
- Uses shadcn Card for consistent styling

**Acceptance Criteria:**
- [ ] Component loads without errors
- [ ] Shows loading state initially
- [ ] Shows empty state when no favorites
- [ ] Shows grid of favorite movies
- [ ] No TypeScript errors

---

#### Task 3.4: Update HomePage

**What to do:**
Add FavoritesSection to main page and update to TypeScript.

**Update `app/page.tsx`:**

```typescript
'use client'

import SearchBar from './components/SearchBar'
import FavoritesSection from './components/FavoritesSection'

export default function Home() {
  return (
    <div className="min-h-screen bg-white p-4">
      <header className="mb-8 border-b pb-4">
        <h1 className="text-4xl font-bold mb-2">🎬 Movie Explorer</h1>
        <p className="text-gray-600">Search millions of movies and save your favorites</p>
      </header>

      {/* Favorites Section - Shows saved movies */}
      <FavoritesSection />

      {/* Search Section */}
      <section>
        <h2 className="text-2xl font-bold mb-4">🔍 Search Movies</h2>
        <SearchBar />
      </section>
    </div>
  )
}
```

**Changes:**
- Renamed from `.js` to `.tsx`
- Added FavoritesSection component
- Updated background to white (matches design)
- Shows favorites prominently at top

**Acceptance Criteria:**
- [ ] HomePage displays correctly
- [ ] FavoritesSection shows at top
- [ ] SearchBar shows below
- [ ] Favorites appear as user adds them
- [ ] No TypeScript errors

---

#### Task 3.5: Test Favorites Feature

**Testing Steps:**
1. Start dev server: `bun dev`
2. Search for "batman"
3. Click heart on a movie → button changes color
4. Refresh page → favorite still shows
5. Add 3-4 more favorites
6. Check FavoritesSection → shows all added movies
7. Click heart again → removes from favorites

**What Should Happen:**
- ✅ Button toggles between Add/Remove
- ✅ Button color changes (default → destructive red)
- ✅ Favorites appear in FavoritesSection
- ✅ Favorites persist after refresh
- ✅ Removing favorite removes from section
- ✅ Count updates correctly
- ✅ No console errors

**If Errors:**
- Check browser console (F12)
- Verify all files created: `.ts` not `.js`
- Verify shadcn components installed
- Check that `lib/types.ts` exists

---

#### Task 3.6: Commit to GitHub

```bash
# Check what changed
git status

# Stage all changes
git add .

# Commit with clear message
git commit -m "Day 3: Add favorites feature with TypeScript hook and localStorage"

# Push to GitHub
git push
```

---

### Day 3 Checklist

- [ ] useFavorites hook created (useFavorites.ts)
- [ ] Hook uses TypeScript interfaces
- [ ] Hook handles localStorage safely
- [ ] MovieCard integrated with hook
- [ ] Favorite button toggles state
- [ ] FavoritesSection component created
- [ ] HomePage updated to include FavoritesSection
- [ ] Favorites persist after page refresh
- [ ] Favorites count displays correctly
- [ ] No TypeScript errors
- [ ] All code committed to GitHub

---

## ✨ Day 4: Loading States & Error Handling

**Time Estimate**: 2-3 hours
**Goals**: Add professional loading skeleton, error handling, and polish UI

### Learning Objectives

By end of Day 4, you should understand:
- Creating loading skeleton components
- Error handling with retry functionality
- Responsive design with Tailwind
- Better UX with visual feedback

### Prerequisites

Make sure you have shadcn/ui Skeleton and Alert components installed:

```bash
bun x shadcn-ui@latest add skeleton
bun x shadcn-ui@latest add alert
```

### Tasks

#### Task 4.1: Create Movie Skeleton Component

**What to do:**
Build a skeleton loader for loading state.

**Create `app/components/MovieSkeleton.tsx`:**

```typescript
'use client'

import { Card, CardContent, CardHeader } from '@/components/ui/card'
import { Skeleton } from '@/components/ui/skeleton'

export default function MovieSkeleton() {
  return (
    <Card className="overflow-hidden">
      {/* Image Skeleton */}
      <CardHeader className="p-0">
        <Skeleton className="h-64 w-full rounded-none" />
      </CardHeader>

      {/* Content Skeleton */}
      <CardContent className="p-4">
        {/* Title Skeleton */}
        <Skeleton className="h-6 w-full mb-3" />

        {/* Rating and Year Skeleton */}
        <div className="flex gap-2 mb-3">
          <Skeleton className="h-6 w-20" />
          <Skeleton className="h-6 w-16" />
        </div>

        {/* Description Skeleton */}
        <Skeleton className="h-4 w-full mb-2" />
        <Skeleton className="h-4 w-5/6 mb-4" />

        {/* Button Skeleton */}
        <Skeleton className="h-10 w-full" />
      </CardContent>
    </Card>
  )
}
```

**What This Code Does:**
- Uses shadcn/ui Skeleton component for professional loading animation
- Matches MovieCard layout exactly
- Automatically animates with pulsing effect
- Responsive and accessible

**Acceptance Criteria:**
- [ ] Skeleton component created
- [ ] Matches MovieCard layout
- [ ] Smooth pulsing animation
- [ ] Responsive to screen size
- [ ] No TypeScript errors

---

#### Task 4.2: Update SearchBar with Loading State

**What to do:**
Show skeleton loaders while searching.

**Update `app/components/SearchBar.tsx`:**

Add import at top:
```typescript
import MovieSkeleton from './MovieSkeleton'
```

Update your return statement to show skeletons during loading:

```typescript
      {/* Loading Skeletons */}
      {state.loading && (
        <div className="mt-8">
          <p className="text-gray-600 mb-4">Searching for movies...</p>
          <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
            {Array.from({ length: 8 }).map((_, index) => (
              <MovieSkeleton key={`skeleton-${index}`} />
            ))}
          </div>
        </div>
      )}

      {/* Results Grid */}
      {!state.loading && state.results.length > 0 && (
        <div className="mt-8">
          <h2 className="text-2xl font-bold mb-4">
            Found {state.results.length} movies
          </h2>
          <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
            {state.results.map((movie) => (
              <MovieCard key={movie.id} movie={movie} />
            ))}
          </div>
        </div>
      )}
```

**Acceptance Criteria:**
- [ ] Skeletons show while searching
- [ ] 8 skeletons display in grid
- [ ] Skeletons disappear when results load
- [ ] Smooth animation transition
- [ ] No TypeScript errors

---

#### Task 4.3: Add Error Handling with Retry

**What to do:**
Show errors with retry button using shadcn/ui Alert.

**Update `app/components/SearchBar.tsx`:**

Add import:
```typescript
import { Alert, AlertDescription } from '@/components/ui/alert'
```

Update error display in return statement:

```typescript
      {/* Error Alert */}
      {state.error && (
        <Alert variant="destructive" className="mb-4 flex items-center justify-between">
          <div className="flex-1">
            <AlertDescription>
              {state.error}
            </AlertDescription>
          </div>
          <Button
            onClick={async () => {
              // Retry the last search
              if (state.query) {
                setState(prev => ({ ...prev, error: '' }))
                // Search will be triggered by form
              }
            }}
            variant="outline"
            size="sm"
            className="ml-2"
          >
            Retry
          </Button>
        </Alert>
      )}
```

**What This Does:**
- Shows error in shadcn Alert component
- Provides Retry button for user
- Clears error when user retries
- Professional error display

**Acceptance Criteria:**
- [ ] Error displays with message
- [ ] Retry button appears and works
- [ ] Error clears on retry
- [ ] No TypeScript errors

---

#### Task 4.4: Improve HomePage Layout

**What to do:**
Add sticky header and better responsive design.

**Update `app/page.tsx`:**

```typescript
'use client'

import SearchBar from './components/SearchBar'
import FavoritesSection from './components/FavoritesSection'

export default function Home() {
  return (
    <div className="min-h-screen bg-gradient-to-b from-white to-gray-50">
      {/* Sticky Header */}
      <header className="sticky top-0 z-50 bg-white/95 backdrop-blur border-b border-gray-200 shadow-sm">
        <div className="max-w-6xl mx-auto px-4 py-4">
          <h1 className="text-3xl md:text-4xl font-bold">🎬 Movie Explorer</h1>
          <p className="text-sm md:text-base text-gray-600">
            Search millions of movies and save your favorites
          </p>
        </div>
      </header>

      {/* Main Content */}
      <main className="max-w-6xl mx-auto px-4 py-8 md:py-12">
        {/* Favorites Section */}
        <FavoritesSection />

        {/* Search Section */}
        <section className="mt-12">
          <h2 className="text-2xl md:text-3xl font-bold mb-6">🔍 Search Movies</h2>
          <SearchBar />
        </section>
      </main>

      {/* Footer */}
      <footer className="border-t border-gray-200 mt-12 py-6">
        <div className="max-w-6xl mx-auto px-4 text-center text-gray-600 text-sm">
          <p>
            Movie data from{' '}
            <a href="https://www.themoviedb.org/" className="font-semibold hover:text-gray-900">
              TMDB API
            </a>
            {' | '}Built with Next.js, React, TypeScript & shadcn/ui
          </p>
        </div>
      </footer>
    </div>
  )
}
```

**What Changed:**
- Renamed from `.js` to `.tsx`
- Added sticky header that stays on scroll
- Better responsive padding/spacing
- Gradient background
- Professional footer with links
- Backdrop blur effect

**Acceptance Criteria:**
- [ ] Header sticks on scroll
- [ ] Works on mobile (375px)
- [ ] Works on tablet (768px)
- [ ] Works on desktop (1024px+)
- [ ] Text sizes scale properly
- [ ] No TypeScript errors

---

#### Task 4.5: Test Loading & Errors

**Testing Steps:**
1. Start dev server: `bun dev`
2. Search for "batman" → should show 8 skeletons then results
3. Open DevTools → Throttle network to "Slow 3G"
4. Search again → watch skeletons load slowly
5. Test error: Disconnect internet
6. Try to search → should show error with Retry button
7. Reconnect internet and click Retry
8. Test responsive: Resize browser or use mobile device

**What Should Happen:**
- ✅ Skeletons load smoothly
- ✅ Results replace skeletons
- ✅ Error shows with Retry button
- ✅ Retry works correctly
- ✅ Header stays sticky
- ✅ Responsive on all sizes
- ✅ No console errors

**If Errors:**
- Check browser console (F12)
- Verify `MovieSkeleton.tsx` created
- Verify shadcn Skeleton installed: `ls app/components/ui/skeleton.tsx`
- Check that SearchBar uses `state.loading` not `loading`

---

#### Task 4.6: Commit to GitHub

```bash
# Check changes
git status

# Stage all
git add .

# Commit with clear message
git commit -m "Day 4: Add loading skeleton, error handling, and improve layout"

# Push
git push
```

---

### Day 4 Checklist

- [ ] MovieSkeleton component created (MovieSkeleton.tsx)
- [ ] SearchBar imports and uses MovieSkeleton
- [ ] Skeletons display during loading
- [ ] Error Alert with Retry button works
- [ ] HomePage has sticky header
- [ ] Layout responsive (mobile, tablet, desktop)
- [ ] Gradient background applied
- [ ] Footer displays correctly
- [ ] Loading and error states tested
- [ ] All code is TypeScript (.tsx)
- [ ] No TypeScript errors
- [ ] All code committed to GitHub

---

## ✨ Day 5: Final Polish & Code Quality

**Time Estimate**: 2-3 hours
**Goals**: Add accessibility, improve TypeScript types, test thoroughly, clean up

### Learning Objectives

By end of Day 5, you should understand:
- Accessibility best practices (a11y)
- TypeScript strict mode and type safety
- Testing strategies
- Code quality and best practices
- Production-ready code

### Tasks

#### Task 5.1: Add Accessibility Features

**What to do:**
Make app accessible to users with disabilities.

**Update `app/components/MovieCard.tsx`:**

Add aria-labels to the Button component:

```typescript
<Button
  onClick={handleToggleFavorite}
  variant={isLiked ? 'destructive' : 'default'}
  className="w-full"
  aria-label={isLiked ? `Remove ${movie.title} from favorites` : `Add ${movie.title} to favorites`}
>
  {isLiked ? '❤️ Remove from Favorites' : '🤍 Add to Favorites'}
</Button>
```

**Update `app/components/SearchBar.tsx`:**

Add aria-labels to Input and Button:

```typescript
<Input
  type="text"
  value={state.query}
  onChange={(e) => setState(prev => ({ ...prev, query: e.target.value }))}
  placeholder="Search for a movie..."
  aria-label="Search movies by title"
/>
<Button
  type="submit"
  disabled={state.loading}
  aria-busy={state.loading}
>
  {state.loading ? 'Searching...' : 'Search'}
</Button>
```

**Acceptance Criteria:**
- [ ] Images have descriptive alt text
- [ ] Buttons have aria-labels
- [ ] Form inputs have aria-labels
- [ ] Loading state has aria-busy
- [ ] Keyboard navigation works (Tab key)
- [ ] Color contrast meets WCAG AA standard
- [ ] Focus indicators visible on interactive elements

---

#### Task 5.2: Review and Improve Types

**What to do:**
Ensure all components have proper TypeScript types.

**Checklist for TypeScript Best Practices:**

```typescript
// ✅ Good - Props are typed
interface MovieCardProps {
  movie: Movie
}

export default function MovieCard({ movie }: MovieCardProps) {
  // ...
}

// ❌ Bad - Props are not typed
export default function MovieCard({ movie }) {
  // ...
}
```

**Review all your files:**
- [ ] All component props have interfaces
- [ ] All state variables have types
- [ ] All function return types are specified
- [ ] No `any` types used (except where absolutely necessary)
- [ ] API responses have types
- [ ] Event handlers have proper typing

**Files to review:**
- `app/components/SearchBar.tsx` - Check state, handlers
- `app/components/MovieCard.tsx` - Check props interface
- `app/hooks/useFavorites.ts` - Check return type interface
- `app/api/search/route.ts` - Check request/response types
- `lib/types.ts` - Check all exported types are complete

**Acceptance Criteria:**
- [ ] No TypeScript errors in entire project
- [ ] No `any` types (except in unavoidable cases)
- [ ] All exports have proper types
- [ ] All props have interfaces
- [ ] `tsc --noEmit` passes without errors

---

#### Task 5.3: Comprehensive Testing

**What to do:**
Test all features thoroughly before deployment.

**Desktop Testing (Chrome/Firefox/Safari):**
```
Functionality:
[ ] Search returns results
[ ] Results display in grid (1, 2, 3, or 4 columns correctly)
[ ] Movie cards show: title, poster, rating, year, description
[ ] Add to favorites works
[ ] Remove from favorites works
[ ] Favorites persist after page refresh

UI/UX:
[ ] Header stays sticky when scrolling
[ ] Loading skeletons display during search
[ ] Error message shows with Retry button
[ ] Smooth transitions and animations
[ ] No console errors (F12)
```

**Mobile Testing (iPhone/Android viewport):**
```
Responsiveness:
[ ] Works at 375px (mobile)
[ ] Works at 768px (tablet)
[ ] Grid adapts: 1 col mobile → 4 cols desktop
[ ] Text is readable (no zoom needed)
[ ] Buttons are large enough to tap
[ ] No horizontal scrollbar

Touch:
[ ] All buttons are tappable (min 48px)
[ ] Search works on mobile keyboard
[ ] Scrolling is smooth
[ ] No layout shift on scroll
```

**Network Testing:**
```
[ ] Test on fast network (loads in <2s)
[ ] Test on slow 3G (verify skeletons show)
[ ] Test on offline (error displays, retry works)
[ ] Test after reconnect (retry button works)
```

**Acceptance Criteria:**
- [ ] All features work on desktop
- [ ] All features work on mobile
- [ ] All features work on tablet
- [ ] No errors in browser console
- [ ] Lighthouse score >90 for Performance
- [ ] No TypeScript errors

---

#### Task 5.4: Code Cleanup

**What to do:**
Clean up code, remove debug statements, and optimize.

**Checklist:**
- [ ] Remove all `console.log()` statements (keep only `console.error()`)
- [ ] Remove unused imports
- [ ] Remove unused variables
- [ ] Add JSDoc comments to complex functions
- [ ] Check for unused CSS classes

**Run linting:**
```bash
# Check for issues
bun run lint

# Auto-fix simple issues
bun run lint -- --fix
```

**Example of good comments:**
```typescript
/**
 * Loads movie favorites from localStorage on component mount
 * Handles errors gracefully and continues without data if loading fails
 */
useEffect(() => {
  try {
    // Only run in browser
    if (typeof window !== 'undefined') {
      // ...
    }
  } catch (error) {
    console.error('Failed to load favorites:', error)
  }
}, [])
```

**Acceptance Criteria:**
- [ ] No console.log statements (except errors)
- [ ] No unused imports
- [ ] No unused variables
- [ ] Complex code has comments
- [ ] Linter passes (`bun run lint`)
- [ ] All code follows TypeScript strict mode

---

#### Task 5.5: Update Project README

**What to do:**
Create comprehensive README for your project repository.

**Update/Create `README.md` in project root:**

```markdown
# 🎬 Movie Explorer

A modern movie search app built with Next.js, TypeScript, and shadcn/ui.

**Live Demo**: [your-username.vercel.app](https://your-username.vercel.app)

## Features

- 🔍 **Search Movies** - Search millions of movies from TMDB
- 📸 **Movie Details** - View posters, ratings, release dates, and descriptions
- ❤️ **Save Favorites** - Bookmark movies (saves locally in browser)
- 📱 **Responsive Design** - Works perfectly on mobile, tablet, and desktop
- ⚡ **Fast Performance** - Built with Next.js for optimal speed
- 🎨 **Modern UI** - Professional components from shadcn/ui

## Tech Stack

- **Frontend**: React 18 + Next.js 14
- **Language**: TypeScript (strict mode)
- **Styling**: Tailwind CSS + shadcn/ui
- **API**: TMDB (The Movie Database)
- **Storage**: Browser localStorage
- **Deployment**: Vercel

## Getting Started

### Prerequisites
- **Node.js** v20 or higher
- **Bun** v1.1 or higher ([install bun](https://bun.sh))
- **TMDB API Key** (free, get it [here](https://www.themoviedb.org/settings/api))

### Installation

1. **Clone and navigate to project**
   ```bash
   git clone https://github.com/YOUR_USERNAME/movie-explorer.git
   cd movie-explorer
   ```

2. **Install dependencies**
   ```bash
   bun install
   ```

3. **Create environment file** (`.env.local`)
   ```bash
   NEXT_PUBLIC_TMDB_API_KEY=your_api_key_here
   NEXT_PUBLIC_TMDB_BASE_URL=https://api.themoviedb.org/3
   ```

4. **Start development server**
   ```bash
   bun dev
   ```

5. **Open in browser**
   ```
   http://localhost:3000
   ```

## Project Structure

```
app/
├── components/          # React components
│   ├── SearchBar.tsx    # Search input and logic
│   ├── MovieCard.tsx    # Movie display card
│   ├── MovieSkeleton.tsx # Loading skeleton
│   └── FavoritesSection.tsx # Favorites display
├── api/
│   └── search/route.ts  # TMDB search API route
├── hooks/
│   └── useFavorites.ts  # Custom hook for favorites
├── lib/
│   └── types.ts         # TypeScript type definitions
└── page.tsx             # Home page
```

## Key Features Explained

### Search (`app/components/SearchBar.tsx`)
- Type-safe search with TypeScript
- Real-time loading state with skeleton cards
- Error handling with retry button
- Debounced API calls for performance

### Favorites (`app/hooks/useFavorites.ts`)
- Custom React hook for state management
- Automatic localStorage persistence
- Prevents duplicate favorites
- Full TypeScript typing

### API Integration (`app/api/search/route.ts`)
- Next.js API route for TMDB integration
- Type-safe API responses
- Error handling and validation
- Environment variable protection

## What I Learned

- ✅ React hooks (useState, useEffect, custom hooks)
- ✅ TypeScript in React components
- ✅ Next.js App Router and API routes
- ✅ Component composition and props
- ✅ localStorage for data persistence
- ✅ Responsive design with Tailwind CSS
- ✅ Error handling and loading states
- ✅ Git version control and GitHub

## Future Improvements

- [ ] Movie details page with more info
- [ ] Sort and filter options
- [ ] User authentication
- [ ] Share favorites with friends
- [ ] Movie recommendations
- [ ] Dark mode toggle
- [ ] Infinite scroll for results

## Troubleshooting

**Search returns no results?**
- Verify TMDB API key in `.env.local`
- Check if API key has search permissions
- Try searching for popular movies: "avatar", "inception"

**Favorites not saving?**
- Check browser localStorage is enabled (not in private mode)
- Clear cache and try again
- Check browser console for errors (F12)

**App won't start?**
- Verify Node.js v20+: `node --version`
- Verify Bun v1.1+: `bun --version`
- Delete `node_modules` and `.next`: `rm -rf node_modules .next`
- Reinstall: `bun install`

## Testing Checklist

Before deployment, verify:
- [ ] Search functionality works
- [ ] Results display correctly
- [ ] Favorites add/remove works
- [ ] Favorites persist after refresh
- [ ] App works on mobile
- [ ] No console errors
- [ ] Loading states show
- [ ] Error handling works
- [ ] All TypeScript errors fixed

## Deployment

Deploy to Vercel (1-click):

1. Push code to GitHub
2. Go to [vercel.com](https://vercel.com)
3. Create new project from your GitHub repo
4. Add environment variable: `NEXT_PUBLIC_TMDB_API_KEY`
5. Deploy!

[Detailed deployment guide →](../../tasks/04-deployment/deployment.md)

## License

MIT - Feel free to use this project for learning!

## Credits

- Movie data: [TMDB API](https://www.themoviedb.org/)
- UI Components: [shadcn/ui](https://shadcn-ui.com/)
- Framework: [Next.js](https://nextjs.org/)
```

**Acceptance Criteria:**
- [ ] README created/updated
- [ ] Has features section
- [ ] Has tech stack section
- [ ] Has setup instructions
- [ ] Has project structure
- [ ] Has troubleshooting section
- [ ] Clear and well-formatted

---

#### Task 5.6: Final Testing & Commit

**Testing:**
```bash
# Run type check
tsc --noEmit

# Run linting
bun run lint

# Run app
bun dev
# Test all features in browser
```

**Final Commit:**
```bash
# Check changes
git status

# Stage all
git add .

# Commit
git commit -m "Day 5: Add accessibility, improve types, final cleanup"

# Push
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
