# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Added
- 🧲 **Integrated Magnet Support**: Added `magneturl` field to main indexer
  - Works with Prowlarr's "Prefer Magnet URL" option
  - Avoids 401 errors on .torrent download when enabled
  - ⚠️ Magnet links do NOT work for cross-seeding
- 📚 **API Documentation**: New `API_DOCUMENTATION.md` file in English
  - Complete documentation of `/api/external`, `/api/external/meta`, `/api/external/upload` endpoints
  - Category list, error codes, cURL examples
- � **CHANGELOG.md**: Added changelog file
- 💎 **TorBox Sponsor**: Added TorBox sponsor in README
- 📦 New "Other" category with `divers` slug
- ℹ️ New `info_magnet` setting with magnet usage instructions
- ℹ️ New `info_updates` setting with link to GitHub repo

### Changed
- 🏷️ **Updated categories**: 22 categories based on real `/api/external/meta` endpoint
  - Video: `films`, `series`, `spectacles`
  - Audio: `music`, `audio-divers`
  - Games: `pc`, `consoles`, `jeux-mobiles`
  - Applications: `systemes`, `software`
  - E-books: `romans`, `bd`, `documentaires`, `livres`, `presse`, `education`
  - XXX: `xxx-hetero`, `xxx-gay`, `xxx-lesbien`, `xxx-trans` (disabled by default)
  - Other: `divers`
- 📝 **README completely redesigned**:
  - Simplified installation (single file)
  - Category tables with real API slugs
  - Link to API documentation
  - TorBox sponsor section
  - Contributors section
- 🔧 **Improved indexer**:
  - Added `_id` field (guid)
  - Added `magneturl` field for magnet support
  - Fixed date parsing with proper ISO 8601 format
  - Enabled category filtering with `cat`
  - Added `requestDelay: 2` to avoid rate limiting
  - Updated `info_flaresolverr` to mention both FlareSolverr and Byparr

### Fixed
- 🐛 Fixed trailing slash in base URL (PR #8)
- 🐛 Fixed date parsing format (was incorrect MM/dd/yyyy, now proper ISO 8601)

### Removed
- 🗑️ Removed separate `lacale-api-magnet.yml` file (merged into main indexer)

---

## [0.2.0] - 2026-01-01

### Added
- ⚠️ Cloudflare warning in README and indexer
- 🎁 Global Freeleech and Double Upload active indicators
- ℹ️ New `info_flaresolverr` field in settings
- ℹ️ New `info_ratio` field with ratio rules

### Changed
- 🔗 Fixed passkey link (now points to `/profile`)
- 📊 `downloadvolumefactor: 0` (Freeleech)
- 📊 `uploadvolumefactor: 2` (Double Upload)

### Fixed
- 🐛 Added trailing slash to base URL (PR #8)

---

## [0.1.3] - 2025-12-30

### Added
- 📡 `API_IMPROVEMENTS.md`: Improvement suggestions for La Cale API
  - Pagination, sorting, advanced filters
  - Missing fields (grabs, files, imdbId)
  - User statistics endpoint

### Changed
- 📝 Updated README with link to API improvements

---

## [0.1.2] - 2025-12-30

### Added
- 📖 `CONTRIBUTING.md`: Contribution guide
- 🛠️ `DEVELOPER.md`: Technical documentation for developers

---

## [0.1.1] - 2025-12-30

### Added
- 💰 GitHub Sponsors configuration
- ☕ Ko-fi configuration
- `FUNDING.yml` file

---

## [0.1.0] - 2025-12-30

### Added
- 🏴‍☠️ **Initial release** of La Cale indexer for Prowlarr
- 🔌 Direct JSON API support (no HTML scraping)
- 🔑 Passkey authentication
- 🔍 Search modes: `search`, `tv-search`, `movie-search`, `music-search`, `book-search`
- 📦 Initial categories:
  - Movies (HD, 4K, Animation)
  - TV Series (HD, VOSTFR)
  - Music (FLAC, MP3)
  - Games (PC, Consoles)
  - Software (Linux, Mac, Windows)
  - Ebooks (Comics, Novels, Documentaries)
  - XXX
- 📝 Pirate-themed README in French
- ⚖️ Ratio rules: minimum 1.0, minimum seed time 48h

---

## Change Types

- `Added` for new features
- `Changed` for changes in existing functionality
- `Deprecated` for soon-to-be removed features
- `Removed` for now removed features
- `Fixed` for any bug fixes
- `Security` for vulnerability fixes

---

[Unreleased]: https://github.com/JigSawFr/lacale-prowlarr-indexer/compare/v0.2.0...HEAD
[0.2.0]: https://github.com/JigSawFr/lacale-prowlarr-indexer/compare/v0.1.3...v0.2.0
[0.1.3]: https://github.com/JigSawFr/lacale-prowlarr-indexer/compare/v0.1.2...v0.1.3
[0.1.2]: https://github.com/JigSawFr/lacale-prowlarr-indexer/compare/v0.1.1...v0.1.2
[0.1.1]: https://github.com/JigSawFr/lacale-prowlarr-indexer/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/JigSawFr/lacale-prowlarr-indexer/releases/tag/v0.1.0
