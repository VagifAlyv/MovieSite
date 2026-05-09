# 🎬 AI Movie Explorer

A beginner JavaScript project that teaches you APIs, AI integration, and JSON — all in one real app. Search for movies, save your favorites, and ask an AI if they're worth watching.

---

## What You'll Learn

| Phase | Skill | Concepts Covered |
|-------|-------|-----------------|
| 1 | Fetch + APIs | `fetch()`, `async/await`, HTTP GET, JSON parsing |
| 2 | JSON + Storage | `JSON.stringify`, `JSON.parse`, `localStorage`, array methods |
| 3 | AI Integration | POST requests, auth headers, OpenAI API, nested JSON |

---

## Project Structure

```
movie-explorer/
├── index.html        ← layout: search bar, results grid, favorites panel
├── style.css         ← card styles, grid, responsive layout
├── app.js            ← Phase 1: fetch TMDB API, render movie cards
├── favorites.js      ← Phase 2: save/load/delete favorites as JSON
├── ai.js             ← Phase 3: POST to OpenAI, display AI review
└── config.js         ← API keys (add this file to .gitignore!)
```

---

## Setup

### 1. Get your API keys

**TMDB (free, no credit card):**
1. Sign up at [themoviedb.org](https://www.themoviedb.org)
2. Go to Settings → API → Create → Developer
3. Copy your API key

**OpenAI (cheap, ~$1–2/month for learning):**
1. Sign up at [platform.openai.com](https://platform.openai.com)
2. Go to API Keys → Create new secret key
3. Copy your key

### 2. Add keys to `config.js`

```js
const TMDB_KEY = 'your_tmdb_key_here';
const OPENAI_KEY = 'your_openai_key_here';
```

> ⚠️ Never push `config.js` to GitHub. Add it to `.gitignore`.

### 3. Open in browser

No server or install needed for Phase 1 and 2. Just open `index.html` directly in your browser.

---

## Phase 1 — Search Movies (TMDB API)

```js
async function searchMovies(query) {
  const url = `https://api.themoviedb.org/3/search/movie?api_key=${TMDB_KEY}&query=${query}`;
  const response = await fetch(url);
  const data = await response.json();   // parse the JSON response
  return data.results;                  // array of movie objects
}
```

**What the JSON response looks like:**

```json
{
  "results": [
    {
      "id": 550,
      "title": "Fight Club",
      "overview": "A ticking-time-bomb insomniac...",
      "vote_average": 8.4,
      "release_date": "1999-10-15",
      "poster_path": "/pB8BM7pdSp6B6Ih7QZ4DrQ3PmJK.jpg"
    }
  ]
}
```

---

## Phase 2 — Save Favorites (JSON + localStorage)

```js
function saveFavorite(movie) {
  const raw = localStorage.getItem('favorites');
  const favs = raw ? JSON.parse(raw) : [];        // parse stored JSON

  favs.push({
    id: movie.id,
    title: movie.title,
    rating: movie.vote_average,
    saved_at: new Date().toISOString()
  });

  localStorage.setItem('favorites', JSON.stringify(favs));  // save as JSON string
}
```

**Tip:** Open DevTools → Application → Local Storage to see your JSON data live as you save movies.

---

## Phase 3 — Ask AI (OpenAI API)

```js
async function askAI(movieTitle, overview) {
  const response = await fetch('https://api.openai.com/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${OPENAI_KEY}`
    },
    body: JSON.stringify({
      model: 'gpt-4o-mini',
      messages: [
        { role: 'system', content: 'You are a fun movie critic. Keep answers under 3 sentences.' },
        { role: 'user', content: `Should I watch "${movieTitle}"? Plot: ${overview}` }
      ]
    })
  });

  const data = await response.json();
  return data.choices[0].message.content;   // navigate the nested JSON
}
```

---

## Features to Build

- [x] Search movies by title
- [x] Show title, rating, release date, and poster
- [x] Save favorites to localStorage
- [x] Remove favorites
- [x] Ask AI "Is this worth watching?"
- [ ] Filter favorites by genre
- [ ] Export favorites as a `.json` file download
- [ ] "Recommend me something" AI button based on your saved list
- [ ] Deploy to GitHub Pages or Netlify

---

## JavaScript Concepts Checklist

Work through these as you build each phase:

**Phase 1**
- [ ] `async` / `await`
- [ ] `fetch()` and HTTP GET
- [ ] `response.json()` — parsing a JSON response
- [ ] Template literals (`` `Hello ${name}` ``)
- [ ] DOM manipulation (`getElementById`, `innerHTML`)
- [ ] Array `.map()` to render lists

**Phase 2**
- [ ] `JSON.stringify()` — object to string
- [ ] `JSON.parse()` — string to object
- [ ] `localStorage.setItem` / `getItem`
- [ ] Array `.find()`, `.filter()`, `.push()`

**Phase 3**
- [ ] `fetch()` with POST method
- [ ] Request headers (`Content-Type`, `Authorization`)
- [ ] Sending a JSON body
- [ ] Navigating deeply nested JSON (`data.choices[0].message.content`)
- [ ] Error handling with `try / catch`

---

## Useful Resources

- [TMDB API Docs](https://developer.themoviedb.org/docs)
- [OpenAI API Docs](https://platform.openai.com/docs)
- [MDN — fetch()](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [MDN — async/await](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Promises)
- [MDN — localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

---

## Recommended Build Order

```
Week 1  →  HTML skeleton + CSS layout
Week 2  →  Phase 1: search + display movies
Week 3  →  Phase 2: save/delete favorites
Week 4  →  Phase 3: AI review feature
Week 5  →  Polish + deploy to GitHub Pages
```

Build one phase completely before moving to the next. Each phase works independently.

---

## Security Notes

- Never hardcode API keys in files you push to GitHub
- Add `config.js` to your `.gitignore`
- For production apps, API calls should go through a backend server so keys stay hidden
- For learning purposes, browser-direct calls are fine

---

*Built to learn JavaScript through real projects — APIs, AI, and JSON all in one.*