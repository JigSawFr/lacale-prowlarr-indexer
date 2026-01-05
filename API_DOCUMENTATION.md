# La Cale API Documentation

This document describes the external API provided by La Cale tracker for integration with indexers like Prowlarr and Jackett.

## Base URL

```
https://la-cale.space
```

## Authentication

All requests must include your **Passkey** as a query parameter. You can find your passkey on your [profile page](https://la-cale.space/profile).

> ⚠️ **Never share your passkey!** It provides full access to your account.

```
GET /api/external?passkey=YOUR_PASSKEY
```

### Rate Limiting

- Protection is active by IP and by passkey
- Returns `429 Too Many Requests` if rate limit is exceeded
- Upload endpoint: max 30 requests/minute

---

## Endpoints

### 1. Search Torrents

Search for torrents in the tracker database.

```
GET /api/external
```

#### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `passkey` | Yes | Your secret API key |
| `q` | No | Search query (max 200 characters, normalized: lowercase, accents removed) |
| `cat` | No | Category slug (can be repeated, e.g., `&cat=films&cat=series`) |

#### Response

Returns a JSON array with up to **50 results**, sorted by date descending. Only **approved torrents** are returned.

```json
[
  {
    "title": "Movie Title 2026 1080p BluRay x264",
    "guid": "abc123xyz",
    "size": 4294967296,
    "pubDate": "2026-01-01T12:00:00.000Z",
    "link": "https://la-cale.space/torrents/abc123xyz",
    "category": "Films HD",
    "seeders": 42,
    "leechers": 5,
    "infoHash": "a1b2c3d4e5f6..."
  }
]
```

#### Field Schema

| Field | Type | Description |
|-------|------|-------------|
| `title` | string | Torrent name |
| `guid` | string | Internal torrent ID |
| `size` | number | File size in bytes |
| `pubDate` | string | Publication date (ISO 8601 format with milliseconds) |
| `link` | string | Torrent details page URL |
| `category` | string | Category name (e.g., "Films HD", "Séries TV") |
| `seeders` | number | Number of seeders |
| `leechers` | number | Number of leechers |
| `infoHash` | string | Torrent info hash (hex, used for download endpoint) |

#### Download Endpoint

To download a torrent file, use the infoHash:

```
GET /api/torrents/download/{infoHash}?passkey=YOUR_PASSKEY
```

#### Caching

Server-side cache: ~30 seconds per combination of passkey + query + category.

---

### 2. Get Metadata (Categories & Tags)

Retrieve all available categories, subcategories, and tags for uploads.

```
GET /api/external/meta
```

#### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `passkey` | Yes | Your secret API key |

#### Response

```json
{
  "categories": [
    {
      "id": "cmjudvb9d0000oqrult6eafdv",
      "name": "Vidéo",
      "slug": "video",
      "icon": "Film",
      "parentId": null,
      "children": [
        {
          "id": "cmjoyv2cd00027eryreyk39gz",
          "name": "Films",
          "slug": "films",
          "icon": null,
          "parentId": "cmjudvb9d0000oqrult6eafdv"
        }
      ]
    }
  ],
  "tagGroups": [
    {
      "id": "cmjuhly080002v4ruzrvjf2ds",
      "name": "Genres",
      "slug": "genres",
      "order": 30,
      "tags": [
        {
          "id": "cmjudwh76000guyrus1jc9jxs",
          "name": "Action",
          "slug": "action"
        }
      ]
    }
  ],
  "ungroupedTags": []
}
```

#### Usage Notes

- `categoryId` for uploads comes from `categories[].id` or `categories[].children[].id`
- Tags expect IDs (not slugs) from `tagGroups[].tags[].id`
- Use subcategory slugs for the `cat` parameter in search

---

### 3. Upload Torrent

Upload a new torrent to the tracker via API.

```
POST /api/external/upload
Content-Type: multipart/form-data
```

#### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `passkey` | string | Your secret API key |
| `title` | string | Torrent title |
| `categoryId` | string | Category ID (from `/meta` endpoint) |
| `file` | file | The .torrent file |

#### Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `description` | string | Torrent description |
| `tmdbId` | string | TMDB ID for movies/TV |
| `tmdbType` | string | `MOVIE` or `TV` |
| `coverUrl` | string | Direct URL to cover image (https) |
| `tags` | string[] | Tag IDs (repeat field for multiple: `tags=ID1&tags=ID2`) |
| `nfoFile` | file | NFO file |

#### Response

```json
{
  "success": true,
  "id": "abc123",
  "slug": "movie-title-abc123",
  "link": "https://la-cale.space/torrents/movie-title-abc123"
}
```

#### Important Notes

- **Rate limit**: 30 requests/minute (returns `429` if exceeded)
- **Source flag**: The torrent must contain the source flag `lacale`. Otherwise, clients will need to re-download the torrent from the site.
- **Cover URL**: If `tmdbId` is provided, the cover may be replaced by the TMDB poster.

---

## Available Categories

Based on the `/api/external/meta` endpoint:

### Video
| Slug | Description |
|------|-------------|
| `films` | Films |
| `series` | Séries TV |
| `spectacles` | Spectacles |

### Audio
| Slug | Description |
|------|-------------|
| `music` | Musique |
| `audio-divers` | Audio divers |

### Games
| Slug | Description |
|------|-------------|
| `pc` | PC Games |
| `consoles` | Console Games |
| `jeux-mobiles` | Mobile Games |

### Applications
| Slug | Description |
|------|-------------|
| `systemes` | Operating Systems |
| `software` | Software |

### E-books
| Slug | Description |
|------|-------------|
| `romans` | Novels |
| `bd` | Comics |
| `documentaires` | Documentaries |
| `livres` | Books |
| `presse` | Press/Magazines |
| `education` | Education |

### XXX (Adult)
| Slug | Description |
|------|-------------|
| `xxx-hetero` | XXX Hétéro |
| `xxx-gay` | XXX Gay |
| `xxx-lesbien` | XXX Lesbien |
| `xxx-trans` | XXX Trans |

### Other
| Slug | Description |
|------|-------------|
| `divers` | Miscellaneous |

---

## Current Site Status

The site may have active promotions:

- 🎁 **Freeleech**: Downloads don't count against your ratio
- ⚡ **Double Upload**: Upload credit is doubled

Check the site banner for current promotions.

---

## Error Codes

| Code | Description |
|------|-------------|
| `200` | Success |
| `400` | Bad Request (invalid parameters) |
| `401` | Unauthorized (invalid passkey) |
| `429` | Too Many Requests (rate limited) |
| `500` | Internal Server Error |
| `521` | Cloudflare Origin Down |

---

## Cloudflare Protection

The site may use Cloudflare protection. If you experience issues:

1. Configure **FlareSolverr** in Prowlarr/Jackett settings
2. Use a browser user-agent
3. Respect the rate limits

---

## Integration Examples

### Prowlarr/Jackett

Use the provided indexer definition: `lacale-api-custom.yml`

**Magnet Support**: The indexer includes both `.torrent` download links and magnet links. To use magnets:
1. Go to **Settings → Indexers** in Prowlarr
2. Enable **"Prefer Magnet URL"**

> ⚠️ **Warning**: Magnet links do NOT work for cross-seeding!

### cURL Example

```bash
# Search for torrents
curl "https://la-cale.space/api/external?passkey=YOUR_PASSKEY&q=matrix&cat=films"

# Get categories
curl "https://la-cale.space/api/external/meta?passkey=YOUR_PASSKEY"
```

---

## Support

For issues with the API, contact the La Cale staff through:
- The site's messaging system (Le Pigeonnier)
- The site's forums

For issues with the Prowlarr/Jackett indexer, open an issue on GitHub.
