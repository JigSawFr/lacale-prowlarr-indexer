# 🛠️ Developer Guide

> Complete guide for developing and maintaining the La Cale Prowlarr Indexer.

---

## 📋 Table of Contents

- [📁 Project Structure](#-project-structure)
- [📄 Indexer Definition](#-indexer-definition)
- [🏷️ Category Mapping](#️-category-mapping)
- [🔍 API Reference](#-api-reference)
- [🧪 Testing](#-testing)
- [🔄 Updating the Indexer](#-updating-the-indexer)

---

## 📁 Project Structure

```
lacale-prowlarr-indexer/
├── lacale-api-custom.yml # 🚢 Main indexer definition
├── README.md             # 📖 User documentation (pirate-themed!)
├── CONTRIBUTING.md       # 🤝 Contribution guidelines
├── DEVELOPER.md          # 🛠️ This file
├── LICENSE               # 📜 MIT License
└── .github/
    └── FUNDING.yml       # 💰 Sponsorship links
```

---

## 📄 Indexer Definition

### File: `lacale-api-custom.yml`

The indexer uses the Cardigann v11 format for Prowlarr.

### Key Sections

| Section | Purpose |
|---------|---------|
| `id` | Unique identifier (`lacale-api-custom`) |
| `name` | Display name in Prowlarr |
| `description` | Tracker description |
| `language` | `fr-FR` for French |
| `type` | `private` (requires authentication) |
| `links` | Base URL(s) of the tracker |
| `caps` | Categories and search modes |
| `settings` | User configuration (passkey) |
| `login` | Connection test |
| `search` | API query configuration |

### Response Parsing

The API returns a JSON array directly:

```json
[
  {
    "title": "Movie.Name.2024.1080p...",
    "guid": "unique-id",
    "size": 1234567890,
    "pubDate": "2025-12-29T23:06:41.710Z",
    "link": "https://la-cale.space/api/torrents/download/...",
    "category": "Films HD",
    "seeders": 10,
    "leechers": 2,
    "infoHash": "abc123..."
  }
]
```

### Field Mapping

| API Field | Prowlarr Field | Notes |
|-----------|----------------|-------|
| `title` | `title` | Torrent name |
| `category` | `category` | Maps via `categorymappings` |
| `link` | `download` | Direct .torrent URL |
| `size` | `size` | Size in bytes |
| `pubDate` | `date` | ISO 8601 format |
| `seeders` | `seeders` | Seed count |
| `leechers` | `leechers` | Leech count |
| `infoHash` | `infohash` | Torrent hash |

---

## 🏷️ Category Mapping

### Current Categories

| Category (API) | Slug | Newznab Category |
|----------------|------|------------------|
| Vidéo | `video` | Movies |
| Films | `films` | Movies |
| Films HD | `films-hd` | Movies/HD |
| Films 4K | `films-4k` | Movies/UHD |
| Animation | `animation` | Movies |
| Séries TV | `series` | TV |
| Séries HD | `s-ries-hd` | TV/HD |
| Spectacles | `spectacles` | TV/Other |
| Audio | `audio` | Audio |
| Musique | `music` | Audio |
| FLAC | `flac` | Audio/Lossless |
| MP3 | `mp3` | Audio/MP3 |
| M4A | `m4a` | Audio/Other |
| Audio divers | `audio-divers` | Audio/Other |
| Jeux | `games` | PC/Games |
| PC | `pc` | PC/Games |
| Consoles | `consoles` | Console |
| Jeux mobiles | `jeux-mobiles` | PC/Mobile-Other |
| Applications | `applications` | PC/0day |
| Logiciels | `software` | PC/0day |
| Linux | `linux` | PC/0day |
| Systèmes | `systemes` | PC/ISO |
| E-books | `ebooks` | Books/EBook |
| Romans | `romans` | Books/EBook |
| BD | `bd` | Books/Comics |
| Documentaires | `documentaires` | Books/Technical |
| E-books & Documents | `ebooks-documents` | Books/EBook |
| Livres | `livres` | Books/EBook |
| Presse | `presse` | Books/Mags |
| Éducation | `education` | Books/Technical |
| XXX | `xxx` | XXX |
| Hétéro | `xxx-hetero` | XXX |
| Gay | `xxx-gay` | XXX |
| Lesbien | `xxx-lesbien` | XXX |
| Trans | `xxx-trans` | XXX |
| Autres | `autres` | Other/Misc |
| Divers | `divers` | Other/Misc |

### Adding New Categories

1. Identify the exact category name from the API response
2. Find the appropriate Newznab category from [Newznab Categories](https://wiki.servarr.com/prowlarr/cardigann-yml-definition#categories)
3. Add to `categorymappings`:

```yaml
- {id: "New Category", cat: Appropriate/Category, desc: "New Category"}
```

---

## 🔍 API Reference

### Endpoint

```
GET https://la-cale.space/api/external
```

### Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `passkey` | ✅ | User authentication key |
| `q` | ❌ | Search query |
| `cat` | ❌ | Category filter (comma-separated names) |

### Example Requests

```bash
# Search for "rambo"
curl "https://la-cale.space/api/external?passkey=YOUR_KEY&q=rambo"

# Search in specific categories
curl "https://la-cale.space/api/external?passkey=YOUR_KEY&q=test&cat=Films%20HD,Films%204K"

# Empty search (latest torrents)
curl "https://la-cale.space/api/external?passkey=YOUR_KEY&q="
```

### Response Format

- **Success**: JSON array of torrents (can be empty `[]`)
- **Error**: Likely HTTP error code or empty response

---

## 🧪 Testing

### Quick Test

```bash
# Test API directly
curl "https://la-cale.space/api/external?passkey=YOUR_KEY&q=test"
```

### Prowlarr Testing

1. Install the indexer definition
2. Add indexer with your passkey
3. Click "Test" button
4. Try searches from Prowlarr UI

### Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| Connection failed | Invalid passkey | Verify passkey in profile |
| No results | Empty category | Try different search/category |
| Date errors | Format mismatch | Check `dateparse` format |
| Download fails | Expired link | Verify link format in API |

---

## 🔄 Updating the Indexer

### When to Update

- New categories added to the site
- API endpoint changes
- Bug fixes reported by users
- Prowlarr schema updates

### Update Process

1. 🔍 Identify the change needed
2. 🌿 Create a feature branch
3. ✏️ Modify `lacale-api-custom.yml`
4. 🧪 Test thoroughly
5. 📤 Submit pull request

### Version Compatibility

| Prowlarr Version | Schema Version | Compatible |
|------------------|----------------|------------|
| 1.x | v11 | ✅ |
| 2.x | v11 | ✅ |

---

## 📚 Related Documentation

| Document | Description |
|----------|-------------|
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution guidelines |
| [README.md](README.md) | User documentation |
| [Cardigann Docs](https://wiki.servarr.com/en/prowlarr/cardigann-yml-definition) | Official definition format |
| [Schema v11](https://github.com/Prowlarr/Indexers/blob/master/definitions/v11/schema.json) | JSON schema |

---

## 📜 Tracker Rules Reminder

> ⚠️ Always respect La Cale's rules!

| Rule | Value |
|------|-------|
| Minimum Ratio | 1.0 |
| Minimum Seed Time | 48 hours |

---

## 📄 License

MIT - See [LICENSE](LICENSE)

---

*Last updated: December 2025* 🏴‍☠️
