# 🤝 Contributing Guide

Ahoy, fellow pirate! Thank you for your interest in contributing to La Cale Prowlarr Indexer! 🏴‍☠️

---

## 📋 Table of Contents

- [🚀 Getting Started](#-getting-started)
- [✏️ Making Changes](#️-making-changes)
- [📤 Pull Request Process](#-pull-request-process)
- [📏 Coding Standards](#-coding-standards)
- [🧪 Testing](#-testing)
- [❓ Questions](#-questions)

---

## 🚀 Getting Started

### Prerequisites

| Requirement | Notes |
|-------------|-------|
| Git | Version control |
| Prowlarr | For testing the indexer |
| Text editor | VS Code recommended |
| La Cale account | For testing API calls |

### Fork & Clone

```bash
# Fork the repository on GitHub, then:
git clone https://github.com/YOUR_USERNAME/lacale-prowlarr-indexer.git
cd lacale-prowlarr-indexer
```

---

## ✏️ Making Changes

### 🌿 Branch Naming

| Prefix | Usage |
|--------|-------|
| `feature/` | New features |
| `fix/` | Bug fixes |
| `docs/` | Documentation updates |

### 💬 Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```bash
feat: add new category mapping
fix: resolve date parsing issue
docs: update installation instructions
chore: update dependencies
```

Use pirate emojis for extra flair! 🏴‍☠️

---

## 📤 Pull Request Process

### Workflow

1. 🌿 **Create a feature branch** from `main`
2. ✏️ **Make your changes** following the coding standards
3. 🧪 **Test locally** with Prowlarr
4. 📤 **Push and create a PR**
5. 👀 **Request review** from maintainers

### ✅ PR Checklist

| Check | Description |
|-------|-------------|
| 📋 YAML valid | Indexer definition is valid YAML |
| 🏷️ Categories | Category mappings are correct |
| 🔍 Search | Search functionality works |
| 📚 Docs | Documentation updated if needed |

---

## 📏 Coding Standards

### 📄 YAML Files

| Setting | Value |
|---------|-------|
| Indentation | 2 spaces |
| Encoding | UTF-8 |
| Line endings | LF (Unix) |

### 🏷️ Category Mappings

When adding new categories:

```yaml
# Use the exact category name from the API as ID
- {id: "Category Name", cat: Newznab/Category, desc: "Category Name"}
```

### 📝 Comments

- Use `#` for comments
- Group related categories with header comments
- Keep comments in English

---

## 🧪 Testing

### 🖥️ Local Testing

1. Copy `lacale-api-custom.yml` to Prowlarr's custom definitions folder:
   - **Linux**: `~/.config/Prowlarr/Definitions/Custom/`
   - **Windows**: `%AppData%\Prowlarr\Definitions\Custom\`
   - **Docker**: `/config/Definitions/Custom/`

2. Restart Prowlarr

3. Add the indexer with your passkey

4. Test various searches:
   - Empty search
   - Keyword search
   - Category-filtered search

### ✅ Test Checklist

| Test | Description |
|------|-------------|
| ✅ Connection | Passkey validation works |
| ✅ Search | Returns expected results |
| ✅ Categories | Filtering by category works |
| ✅ Download | Torrent links are valid |
| ✅ Date parsing | Dates display correctly |

---

## ❓ Questions

| Resource | Link |
|----------|------|
| 🐛 Issues | [GitHub Issues](../../issues) |
| 🛠️ Developer Guide | [DEVELOPER.md](DEVELOPER.md) |
| 📚 Prowlarr Docs | [wiki.servarr.com](https://wiki.servarr.com/prowlarr) |
| 📖 Cardigann Docs | [Cardigann Definition](https://wiki.servarr.com/en/prowlarr/cardigann-yml-definition) |

---

## 🙏 Thank You!

Thank you for helping improve La Cale Prowlarr Indexer! Your contributions help the French torrenting community sail smoother seas. ⚓🏴‍☠️

---

*Last updated: December 2025*
