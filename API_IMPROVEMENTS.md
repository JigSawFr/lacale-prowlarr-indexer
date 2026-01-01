# 🚀 API Improvements Roadmap

> Suggested improvements for La Cale API to enable a fully-featured Prowlarr indexer.

This document outlines the API enhancements that would make the indexer optimal for integration with Prowlarr, Radarr, Sonarr, Lidarr, and Readarr.

---

## 📋 Table of Contents

- [🔍 Advanced Search](#-advanced-search)
- [📊 Enriched Data](#-enriched-data)
- [🎁 Economy Features](#-economy-features)
- [📋 Pagination & Sorting](#-pagination--sorting)
- [📡 Enhanced Response](#-enhanced-response)
- [🔐 Authentication & Security](#-authentication--security)
- [📈 Additional Endpoints](#-additional-endpoints)
- [⚠️ Known Issues](#️-known-issues)
- [🏆 Priority Summary](#-priority-summary)

---

## 🔍 Advanced Search

These parameters would enable direct integration with *arr apps (Radarr, Sonarr, etc.):

| Parameter | Example | Description | Used By |
|-----------|---------|-------------|---------|
| `imdbid` | `tt1234567` | Search by IMDB ID | Radarr, Sonarr |
| `tmdbid` | `12345` | Search by TheMovieDB ID | Radarr |
| `tvdbid` | `12345` | Search by TheTVDB ID | Sonarr |
| `tvmazeid` | `12345` | Search by TVMaze ID | Sonarr |
| `traktid` | `12345` | Search by Trakt ID | Radarr, Sonarr |
| `doubanid` | `12345` | Search by Douban ID | Radarr (Asian content) |
| `rageid` | `12345` | Search by TVRage ID | Sonarr (legacy) |
| `season` | `1` | Filter by season number | Sonarr |
| `ep` | `5` | Filter by episode number | Sonarr |
| `year` | `2024` | Filter by release year | Radarr, Sonarr |
| `genre` | `Action` | Filter by genre | All *arr apps |
| `artist` | `Artist Name` | Search by artist name | Lidarr |
| `album` | `Album Name` | Search by album name | Lidarr |
| `label` | `Record Label` | Search by record label | Lidarr |
| `track` | `Track Name` | Search by track name | Lidarr |
| `author` | `Author Name` | Search by book author | Readarr |
| `title` | `Book Title` | Search by book title | Readarr |
| `publisher` | `Publisher` | Search by book publisher | Readarr |

### Example Requests

```bash
# Movie search by IMDB ID
GET /api/external?passkey=XXX&imdbid=tt0462538

# TV search by season/episode
GET /api/external?passkey=XXX&tvdbid=121361&season=1&ep=5

# Music search
GET /api/external?passkey=XXX&artist=Daft%20Punk&album=Discovery
```

---

## 📊 Enriched Data

Additional fields in the API response for better integration:

| Field | Type | Example | Description |
|-------|------|---------|-------------|
| `details` | string | `"/torrent/abc123"` | URL to torrent details page |
| `comments` | string | `"/torrent/abc123#comments"` | URL to comments section |
| `grabs` | integer | `42` | Number of completed downloads |
| `files` | integer | `3` | Number of files in torrent |
| `imdb` | string | `"tt1234567"` | IMDB ID of content |
| `imdbid` | integer | `1234567` | IMDB ID (numeric only) |
| `tmdbid` | integer | `12345` | TMDB ID of content |
| `rageid` | integer | `12345` | TVRage ID (legacy) |
| `tvdbid` | integer | `12345` | TVDB ID of content |
| `tvmazeid` | integer | `12345` | TVMaze ID of content |
| `traktid` | integer | `12345` | Trakt ID of content |
| `doubanid` | integer | `12345` | Douban ID (Asian content) |
| `poster` | string | `"https://..."` | Cover/poster image URL |
| `description` | string | `"Movie synopsis..."` | Content description |
| `genre` | string | `"Action, Thriller"` | Genre(s) |
| `year` | integer | `2024` | Release year |
| `author` | string | `"Author Name"` | Book author (for ebooks) |
| `booktitle` | string | `"Book Title"` | Book title (for ebooks) |
| `publisher` | string | `"Publisher"` | Publisher (for ebooks) |
| `artist` | string | `"Artist Name"` | Artist (for music) |
| `album` | string | `"Album Name"` | Album (for music) |
| `label` | string | `"Record Label"` | Record label (for music) |
| `track` | string | `"Track Name"` | Track name (for music) |

### Enhanced Response Example

```json
{
  "title": "Movie.Name.2024.1080p.BluRay.x265",
  "guid": "abc123",
  "size": 2373640233,
  "pubDate": "2025-12-29T23:06:41.710Z",
  "link": "https://la-cale.space/api/torrents/download/...",
  "category": "Films HD",
  "seeders": 10,
  "leechers": 2,
  "infoHash": "ee70d55fe0fe12e8db34654b2059492d5f57730c",
  "details": "/torrent/abc123",
  "comments": "/torrent/abc123#comments",
  "grabs": 42,
  "files": 3,
  "imdbid": 462538,
  "tmdbid": 7555,
  "tvdbid": null,
  "poster": "https://la-cale.space/images/posters/abc123.jpg",
  "description": "An action-packed movie...",
  "genre": "Action, Adventure",
  "year": 2024,
  "downloadvolumefactor": 1,
  "uploadvolumefactor": 1,
  "minimumratio": 1.0,
  "minimumseedtime": 172800
}
```

---

## 🎁 Economy Features

Fields to indicate special download conditions:

| Field | Type | Example | Description |
|-------|------|---------|-------------|
| `downloadvolumefactor` | float | `0` | Download multiplier (0 = freeleech) |
| `uploadvolumefactor` | float | `2` | Upload multiplier (2 = double upload) |
| `minimumratio` | float | `1.0` | Minimum ratio required before removing |
| `minimumseedtime` | integer | `172800` | Minimum seed time in seconds (48h) |
| `freeleech` | boolean | `true` | Is torrent freeleech? (alternative) |
| `doubleup` | boolean | `true` | Is torrent double upload? (alternative) |

### Values

| `downloadvolumefactor` | Meaning |
|------------------------|---------|
| `0` | 100% Freeleech (no download counted) |
| `0.25` | 75% Freeleech |
| `0.5` | 50% Freeleech |
| `0.75` | 25% Freeleech |
| `1` | Normal (default) |

| `uploadvolumefactor` | Meaning |
|----------------------|---------|
| `0` | No upload credit |
| `1` | Normal upload (default) |
| `2` | Double upload |
| `3` | Triple upload |

---

## 📋 Pagination & Sorting

Parameters for result management:

| Parameter | Type | Example | Description |
|-----------|------|---------|-------------|
| `limit` | integer | `100` | Maximum results to return |
| `offset` | integer | `0` | Skip first N results |
| `page` | integer | `1` | Page number (alternative to offset) |
| `sort` | string | `seeders` | Sort field |
| `order` | string | `desc` | Sort order (asc/desc) |

### Sort Options

| Value | Description |
|-------|-------------|
| `date` | Sort by upload date |
| `seeders` | Sort by seeder count |
| `leechers` | Sort by leecher count |
| `size` | Sort by file size |
| `name` | Sort alphabetically |
| `grabs` | Sort by download count |

### Example

```bash
# Get latest 50 torrents, sorted by seeders
GET /api/external?passkey=XXX&limit=50&sort=seeders&order=desc
```

---

## 📡 Enhanced Response

Wrap results in a metadata object:

### Current Format

```json
[
  {"title": "...", ...},
  {"title": "...", ...}
]
```

### Suggested Format

```json
{
  "status": "success",
  "results": [
    {"title": "...", ...},
    {"title": "...", ...}
  ],
  "total": 150,
  "page": 1,
  "limit": 100,
  "hasMore": true
}
```

### Error Response

```json
{
  "status": "error",
  "error": "Invalid passkey",
  "code": 401
}
```

---

## 🔐 Authentication & Security

### Rate Limiting Headers

| Header | Example | Description |
|--------|---------|-------------|
| `X-RateLimit-Limit` | `100` | Requests allowed per window |
| `X-RateLimit-Remaining` | `95` | Requests remaining |
| `X-RateLimit-Reset` | `1704067200` | Unix timestamp when limit resets |

### API Versioning

```bash
# Versioned endpoint for future compatibility
GET /api/v1/external?passkey=XXX&q=test
```

---

## 📈 Additional Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/categories` | GET | List all categories with IDs |
| `/api/user` | GET | User info (ratio, stats, class) |
| `/api/status` | GET | API health check |
| `/api/rss` | GET | Personal RSS feed |
| `/api/bookmarks` | GET | User's bookmarked torrents |

### Example: Categories Endpoint

```json
GET /api/categories

{
  "categories": [
    {"id": "Films HD", "name": "Films HD", "parent": "Films"},
    {"id": "Films 4K", "name": "Films 4K", "parent": "Films"},
    {"id": "Séries HD", "name": "Séries HD", "parent": "Séries"},
    ...
  ]
}
```

### Example: User Endpoint

```json
GET /api/user?passkey=XXX

{
  "username": "PirateCaptain",
  "class": "Capitaine",
  "ratio": 2.5,
  "uploaded": 107374182400,
  "downloaded": 42949672960,
  "bonus": 1500,
  "warnings": 0,
  "canDownload": true
}
```

---

## ⚠️ Known Issues

### 🔴 Category Filtering Not Working

**Problem:** The `cat` parameter in the API doesn't work - it returns no results when used.

**Impact:** 
- The `cat` parameter is currently disabled in the indexer definition (commented out)
- When Radarr/Sonarr sync with Prowlarr, they request content using category filters
- Since category filtering doesn't work, the sync fails with:
```
No Results in configured categories. See FAQ Entry: Prowlarr will not sync X Indexer to App
```


### 🟠 Cloudflare Protection

**Problem:** During DDoS attacks, La Cale may enable Cloudflare's anti-DDoS protection, which blocks automated API requests from Prowlarr.

**Impact:** Prowlarr receives a `403` or `503` error with a Cloudflare challenge page, causing searches to fail:
```
Cloudflare protection detected for [La Cale (API)], Flaresolverr may be required.
```

**Current Workaround:** 
- In normal conditions, the API endpoint is excluded from Cloudflare protection
- When protection is active, users must configure a proxy like [Byparr](https://github.com/ThePhaseless/Byparr) or [FlareSolverr](https://github.com/FlareSolverr/FlareSolverr) in Prowlarr

**Suggested Fix:** 
- Whitelist API endpoints from Cloudflare protection permanently
- Or provide an alternative API endpoint that bypasses Cloudflare (e.g., via a different subdomain)

---

## 🏆 Priority Summary

### 🔴 Critical (High Impact)

| Feature | Benefit |
|---------|---------|
| `imdbid` parameter | Direct Radarr/Sonarr integration |
| `tmdbid` parameter | Automatic media matching |
| `tvdbid` parameter | TV show identification |
| `details` field | Link to torrent page |
| `season`/`ep` parameters | Episode-specific searches |
| `downloadvolumefactor` field | Freeleech detection |
| `minimumratio`/`minimumseedtime` | Per-torrent rules (if varying) |

### 🟠 Important (Medium Impact)

| Feature | Benefit |
|---------|---------|
| `grabs` field | Popularity indicator |
| `files` field | Number of files in torrent |
| Pagination (`limit`, `page`) | Large result handling |
| `sort`/`order` parameters | Result ordering |
| `year` parameter | Filter by release year |
| `genre` parameter | Filter by genre |

### 🟡 Useful (Nice to Have)

| Feature | Benefit |
|---------|---------|
| `poster` field | Visual display in clients |
| `description` field | Content preview |
| `comments` field | Link to comments page |
| JSON wrapper with `total` | Pagination info |
| `tvmazeid`/`traktid`/`doubanid` | Extended ID support |

### 🟢 Future Enhancements (Music/Books)

| Feature | Benefit |
|---------|---------|
| `artist`/`album`/`label`/`track` | Lidarr integration |
| `author`/`booktitle`/`publisher` | Readarr integration |
| Rate limit headers | Avoid API bans |
| `/api/user` endpoint | User stats in apps |
| `/api/categories` endpoint | Dynamic category loading |

---

## 📊 Schema v11 Complete Field Reference

All fields supported by Prowlarr's Cardigann v11 schema:

### Required Fields (already implemented ✅)

| Field | Status | Description |
|-------|--------|-------------|
| `title` | ✅ | Torrent name |
| `size` | ✅ | Size in bytes |
| `seeders` | ✅ | Seeder count |
| `category` | ✅ | Category name |
| `download` | ✅ | Download URL |

### Optional Fields

| Field | Status | Description |
|-------|--------|-------------|
| `infohash` | ✅ | Torrent hash |
| `leechers` | ✅ | Leecher count |
| `date` | ✅ | Publication date |
| `details` | ⚠️ Fixed `/` | Torrent details page |
| `comments` | ❌ | Comments page URL |
| `description` | ❌ | Torrent description |
| `files` | ❌ | Number of files |
| `grabs` | ❌ | Download count |
| `poster` | ❌ | Cover image URL |
| `genre` | ❌ | Genre(s) |
| `year` | ❌ | Release year |
| `imdb` | ❌ | IMDB URL |
| `imdbid` | ❌ | IMDB ID (numeric) |
| `tmdbid` | ❌ | TMDB ID |
| `tvdbid` | ❌ | TVDB ID |
| `rageid` | ❌ | TVRage ID |
| `tvmazeid` | ❌ | TVMaze ID |
| `traktid` | ❌ | Trakt ID |
| `doubanid` | ❌ | Douban ID |
| `author` | ❌ | Book author |
| `booktitle` | ❌ | Book title |
| `publisher` | ❌ | Publisher |
| `artist` | ❌ | Music artist |
| `album` | ❌ | Music album |
| `label` | ❌ | Record label |
| `track` | ❌ | Track name |
| `downloadvolumefactor` | ⚠️ Fixed `1` | Download multiplier |
| `uploadvolumefactor` | ⚠️ Fixed `1` | Upload multiplier |
| `minimumratio` | ⚠️ Fixed `1.0` | Min ratio |
| `minimumseedtime` | ⚠️ Fixed `172800` | Min seed time |

**Legend:** ✅ Implemented | ⚠️ Hardcoded value | ❌ Not available

---

## 📬 Contact

If you're a La Cale developer and want to discuss these improvements, feel free to:

- 🐛 [Open an issue](../../issues)
- 💬 Contact via the tracker

---

*Last updated: December 2025* 🏴‍☠️
