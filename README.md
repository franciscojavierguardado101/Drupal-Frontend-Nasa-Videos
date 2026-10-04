# Drupal + Frontend — NASA Videos

Full-stack implementation of the NASA Video Library feature, built for a headless Drupal 11 + Next.js architecture. This repo contains both the Drupal backend module and the Next.js frontend component together.

## Structure

```
drupal/     Drupal 11 custom module (francisco_nasa_videos)
frontend/   Next.js component and API proxy route
```

---

## Drupal module (`drupal/`)

Provides the `nasa_videos` paragraph type so editors can place the video library on any page via the CMS.

**What it does:**
- Registers a `nasa_videos` paragraph type via the Paragraphs module
- `field_nasa_v_label` — optional section heading for the video grid
- `field_nasa_v_query` — default search term when the component loads (e.g. "Space", "Mars", "Webb"), configurable per page without touching code
- Delivers config to the Next.js frontend via JSON:API

**Installation:**
```bash
cp -r drupal /path/to/web/modules/custom/francisco_nasa_videos
drush en francisco_nasa_videos -y
drush cr
```

Requires Drupal 10 or 11 with the `paragraphs` module.

---

## Next.js frontend (`frontend/`)

**What it does:**
- Search bar querying the NASA Images and Video Library API
- Topic filter pills: Space, ISS, Mars, Moon, Webb, Earth, Launches, Hubble
- Responsive auto-fill video grid with thumbnails, titles, dates, and descriptions
- Inline HTML5 video modal: fetches the `.mp4` asset URL on open, plays natively in the browser
- ESC key and backdrop click close the modal
- Load More button appends the next page without a page reload

**APIs used (no key required):**

| Data | Source |
|------|--------|
| Video search + metadata | `https://images-api.nasa.gov/search` |
| Video asset (.mp4) URL | `https://images-api.nasa.gov/asset/{nasa_id}` |

**File breakdown:**

| File | Purpose |
|------|---------|
| `frontend/src/app/api/nasa-videos/route.ts` | Next.js API proxy: search and asset URL fetcher |
| `frontend/src/components/paragraphs/nasa-videos/NasaVideos.tsx` | Main component: search, filters, grid, modal |
| `frontend/src/components/paragraphs/nasa-videos/types.ts` | TypeScript interface for NasaVideoItem |
| `frontend/src/components/paragraphs/nasa-videos/index.ts` | Barrel export |

---

## Stack

- Drupal 11 (headless, Pantheon)
- PHP 8.2
- Next.js 16 App Router (Vercel)
- TypeScript
- NASA Images and Video Library API
