<p align="center">
  <img src="https://img.shields.io/badge/Prowlarr-Indexer-blueviolet?style=for-the-badge&logo=prowlarr" alt="Prowlarr Indexer"/>
  <img src="https://img.shields.io/badge/Version-BETA-orange?style=for-the-badge" alt="Beta Version"/>
  <img src="https://img.shields.io/badge/Pavillon-Fran%C3%A7ais-blue?style=for-the-badge" alt="French"/>
</p>

<h1 align="center">🏴‍☠️ La Cale - Prowlarr Indexer</h1>

---

## 💎 Sponsor

<p align="center">
  <a href="https://torbox.app/subscription?referral=da9fde09-a917-4953-9214-93b8a12f0b58">
    <img src="https://torbox.app/assets/logo-bb7a9579.svg" alt="TorBox" height="50"/>
  </a>
  <br/><br/>
  <strong>⚡ Sponsored by <a href="https://torbox.app/subscription?referral=da9fde09-a917-4953-9214-93b8a12f0b58">TorBox</a></strong><br/>
  Premium Torrent & Usenet Cloud Downloader - 80Gbps Speeds
</p>

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Pirata+One&size=24&pause=1000&color=D4AF37&center=true&vCenter=true&width=500&lines=Bienvenue+%C3%A0+bord%2C+moussaillon+!;Hissez+les+torrents+!;Le+butin+vous+attend..." alt="Typing SVG" />
</p>

<p align="center">
  <strong>Indexeur Prowlarr pour <a href="https://la-cale.space/">La Cale</a></strong><br>
  <em>🚢 Un navire français chargé de Films, Séries, Musique, Jeux, Logiciels, Ebooks & XXX</em>
</p>

<p align="center">
  <a href="#-embarquement">Embarquement</a> •
  <a href="#-équipements-du-navire">Équipements</a> •
  <a href="#-cargaison">Cargaison</a> •
  <a href="#-ordres-du-capitaine">Configuration</a> •
  <a href="#-en-cas-de-tempête">Troubleshooting</a> •
  <a href="#-documentation">Documentation</a>
</p>

---

> [!WARNING]
> **⚓ VERSION BETA** — Ce navire est encore en construction dans le chantier naval. Signalez toute voie d'eau en [ouvrant un ticket](../../issues) !

> [!CAUTION]
> **🛡️ PROTECTION CLOUDFLARE** — En cas d'attaque DDoS, La Cale peut activer la protection anti-DDoS de Cloudflare, rendant temporairement l'indexer inopérant. En temps normal, l'API est exclue de cette protection. Si Cloudflare est actif, la seule solution est d'utiliser un proxy comme [Byparr](https://github.com/ThePhaseless/Byparr) ou [FlareSolverr](https://github.com/FlareSolverr/FlareSolverr).

---

## ✨ Équipements du navire

```
    _~^~^~_
\) /  o o  \ (/
  '_   ⏣   _'
  \ '-----' /
```

| Équipement | État |
|:-----------|:----:|
| 🔌 API directe (pas de scraping) | ⚓ |
| 🔑 Authentification par passkey | ⚓ |
| 🏷️ Filtrage par catégorie | ⚓ |
| 📦 Décodage JSON natif | ⚓ |
| 🔍 Recherche multi-modes | ⚓ |
| 🎁 Freeleech global | ⚓ |

---

## ⚓ Embarquement

### Étape 1 — Charger les cartes de navigation

Copiez `lacale-api-custom.yml` dans la cale de Prowlarr :

| Plateforme | Destination |
|:-----------|:------------|
| 🐧 Linux | `~/.config/Prowlarr/Definitions/Custom/` |
| 🪟 Windows | `%AppData%\Prowlarr\Definitions\Custom\` |
| 🐳 Docker | `/config/Definitions/Custom/` |

> **Remarque** : Prowlarr inclut déjà un indexer `lacale-api`. Ce fichier utilise `id: lacale-api-custom` pour éviter le conflit et doit conserver un nom de fichier unique.

### Étape 2 — Relancer le navire

```bash
# Docker - Remettre le navire à flot
docker restart prowlarr

# Systemd - Larguer les amarres
sudo systemctl restart prowlarr
```

### Étape 3 — Rejoindre l'équipage

1. 🧭 Naviguez vers **Indexers** → **Add Indexer**
2. 🔎 Cherchez **"La Cale (API) Custom"**
3. 🔑 Entrez votre **passkey** (trouvable dans votre profil de marin sur la-cale.space)
4. ✅ Cliquez **Test** puis **Save**

---

## 📦 Cargaison

<table>
<tr><th>📚 Bibliothèque du bord</th><th>🎬 Salle de projection</th><th>📺 Quartier des séries</th></tr>
<tr><td>

| Cale | Slug |
|:-----|:-----|
| Romans | `romans` |
| BD | `bd` |
| Documentaires | `documentaires` |
| Livres | `livres` |
| Presse | `presse` |
| Éducation | `education` |

</td><td>

| Cale | Slug |
|:-----|:-----|
| Films | `films` |
| Films HD | `films-hd` |
| Films 4K | `films-4k` |
| Animation | `animation` |
| Spectacles | `spectacles` |

</td><td>

| Cale | Slug |
|:-----|:-----|
| Séries TV | `series` |
| Séries HD | `s-ries-hd` |

</td></tr>
</table>

<table>
<tr><th>🎵 Taverne musicale</th><th>🎮 Salle de jeux</th><th>💻 Arsenal logiciel</th></tr>
<tr><td>

| Cale | Slug |
|:-----|:-----|
| Musique | `music` |
| FLAC | `flac` |
| MP3 | `mp3` |
| M4A | `m4a` |
| Audio divers | `audio-divers` |

</td><td>

| Cale | Slug |
|:-----|:-----|
| PC | `pc` |
| Consoles | `consoles` |
| Jeux mobiles | `jeux-mobiles` |

</td><td>

| Cale | Slug |
|:-----|:-----|
| Systèmes | `systemes` |
| Logiciels | `software` |
| Linux | `linux` |

</td></tr>
</table>

<table>
<tr><th>🔞 Quartier interdit</th><th>📦 Autres</th></tr>
<tr><td>

| Cale | Slug |
|:-----|:-----|
| XXX | `xxx` |
| Hétéro | `xxx-hetero` |
| Gay | `xxx-gay` |
| Lesbien | `xxx-lesbien` |
| Trans | `xxx-trans` |

</td><td>

| Cale | Slug |
|:-----|:-----|
| Autres | `autres` |
| Divers | `divers` |

</td></tr>
</table>

---

## ⚙️ Ordres du Capitaine

| Paramètre | Description |
|:----------|:------------|
| 🔑 **Passkey** | Votre laissez-passer personnel délivré par le capitaine |

### 📜 Code des Pirates

> *"Tout marin qui ne respecte pas le code sera jeté par-dessus bord !"*

| Règle | Sentence |
|:------|:---------|
| ⚖️ Ratio minimum | `1.0` — Donnez autant que vous prenez ! |
| ⏱️ Temps de seed minimum | `48 heures` — Ne quittez pas le navire trop tôt ! |

---

## 🌊 En cas de tempête

<details>
<summary><strong>❌ Le navire refuse de répondre</strong></summary>

```
    ⛈️ TEMPÊTE DÉTECTÉE ⛈️
```

- 🔑 Vérifiez que votre passkey est correcte
- 👤 Assurez-vous que votre compte est actif sur la-cale.space
- 🌐 Vérifiez que le navire est accessible (le site n'est pas en maintenance)

</details>

<details>
<summary><strong>🔍 La cale semble vide</strong></summary>

```
    🏝️ TERRE EN VUE... MAIS RIEN À L'HORIZON
```

- 📝 Essayez une recherche plus large
- 📦 Vérifiez que la cargaison existe dans cette catégorie
- 🎫 Vérifiez vos droits d'accès aux différentes cales

</details>

<details>
<summary><strong>🛡️ Cloudflare bloque le passage</strong></summary>

```
    ⚔️ BOUCLIER ENNEMI DÉTECTÉ
```

Le site peut activer la protection Cloudflare en cas d'attaque DDoS. En temps normal, l'API est exclue de cette protection.

**Solutions :**
- ⏳ Patientez quelques heures, la protection est généralement temporaire
- 🔄 Utilisez un proxy comme [Byparr](https://github.com/ThePhaseless/Byparr) ou [FlareSolverr](https://github.com/FlareSolverr/FlareSolverr)
- ⚙️ Configurez le proxy dans Prowlarr sous **Settings** → **Indexers** → **FlareSolverr**

</details>

---

## 🤝 Rejoindre l'équipage

Tout marin volontaire est le bienvenu ! Vous pouvez :

- 🐛 [Signaler une avarie](../../issues)
- 💡 [Proposer des améliorations](../../issues)
- 🔧 [Soumettre des réparations](../../pulls)
- 📖 [Lire le guide du contributeur](CONTRIBUTING.md)
- 🛠️ [Consulter la doc technique](DEVELOPER.md)

---

## 📚 Documentation

| Document | Description |
|:---------|:------------|
| 📖 [API_DOCUMENTATION.md](API_DOCUMENTATION.md) | Documentation complète de l'API La Cale |
| 📜 [CHANGELOG.md](CHANGELOG.md) | Historique des modifications |
| 📖 [CONTRIBUTING.md](CONTRIBUTING.md) | Guide pour rejoindre l'équipage |
| 🛠️ [DEVELOPER.md](DEVELOPER.md) | Documentation technique |
| 🚀 [API_IMPROVEMENTS.md](API_IMPROVEMENTS.md) | Suggestions d'améliorations pour l'API |

---

## 🏆 Contributeurs

Un grand merci à tous les marins qui ont contribué à ce projet !

Merci également à tous ceux qui ont signalé des bugs, proposé des améliorations ou simplement testé l'indexer. Chaque contribution compte ! 🏴‍☠️

---

## 💰 Soutenir le Capitaine

Si ce navire vous a aidé dans vos aventures, vous pouvez soutenir le capitaine :

<p align="center">
  <a href="https://github.com/sponsors/JigSawFr"><img src="https://img.shields.io/badge/GitHub_Sponsors-💜-ea4aaa?style=for-the-badge" alt="GitHub Sponsors"/></a>
  <a href="https://ko-fi.com/jigsawfr"><img src="https://img.shields.io/badge/Ko--fi-☕-ff5e5b?style=for-the-badge" alt="Ko-fi"/></a>
</p>

---

## ⚠️ Avertissement du Capitaine

> *"Ce navire est fourni tel quel pour un usage personnel. Assurez-vous de respecter le code des pirates de La Cale. Tout contrevenant sera abandonné sur une île déserte."*

---

## 📜 Licence

MIT License — Libre comme l'océan !

---

<p align="center">
  <sub>
    ⚓ Forgé avec ❤️ pour les flibustiers francophones ⚓
  </sub>
</p>

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:1a1a2e,100:16213e&height=100&section=footer" width="100%"/>
</p>
