---
name: divi-site-builder
description: "Création et édition de sites Divi (Elegant Themes). Trois modes : (1) Génération JSON importable depuis un prototype HTML, (2) Édition live via Chrome MCP + REST API, (3) Debug CSS/override. Lire SKILL.md en premier, puis le fichier correspondant au mode de travail."
---

# Divi Site Builder

## Fichiers complémentaires

| Tâche | Fichier |
|---|---|
| Générer un JSON — page standard | `docs/json-generation.md` puis `docs/json-pages/` |
| Générer un JSON — Theme Builder | `docs/json-generation.md` puis `docs/json-theme-builder/conventions.md` |
| Modifier une page live | `docs/live-editing.md` |

### Sous-dossier `docs/json-pages/`
| Fichier | Contenu |
|---|---|
| `conventions.md` | Syntaxe font, border_radii, gradients, hover, spacing |
| `structure.md` | section, row (toutes colonnes), column |
| `modules-base.md` | text, image, button, code, team_member, contact_form, fullwidth_header |
| `modules-woo.md` | Tous les modules WooCommerce + shop |

### Sous-dossier `docs/json-theme-builder/`
| Fichier | Contenu |
|---|---|
| `conventions.md` | Différences vs pages standard uniquement |

---

## Éditions du Sentin — sentin-tools v1.7

### Principe
Ne jamais poser de questions sur le CSS/JS/PHP/contenu des pages — lire directement via les endpoints REST. Tous les endpoints sont accessibles sans auth depuis une session admin connectée (nonce via `window.wpApiSettings.nonce` ou `/wp-admin/admin-ajax.php?action=rest-nonce`).

### Endpoints lecture (GET)

| Ressource | Endpoint |
|---|---|
| CSS global Divi | `GET /wp-json/sentin/v1/css` |
| JS global Divi | `GET /wp-json/sentin/v1/js` |
| PHP thème enfant | `GET /wp-json/sentin/v1/php` |
| post_content brut d'une page/layout | `GET /wp-json/sentin/v1/page?id=XXX` |
| post_content + contenu décodé | `GET /wp-json/sentin/v1/page-full?id=XXX` |
| Metas Divi d'une page | `GET /wp-json/sentin/v1/page-meta?id=XXX` |
| **Modules et_pb_code décodés** | `GET /wp-json/sentin/v1/code-modules?id=XXX` |
| **Tous les layouts Theme Builder** | `GET /wp-json/sentin/v1/all-layouts` |
| Source du plugin sentin-tools | `GET /wp-json/sentin/v1/plugin` (auth manage_options) |
| Option WP par clé | `GET /wp-json/sentin/v1/option?key=XXX` (auth manage_options) |

### Endpoints écriture (POST)

| Action | Endpoint |
|---|---|
| Régénérer snapshot CSS | `POST /wp-json/sentin/v1/css-file` |
| Régénérer snapshot JS | `POST /wp-json/sentin/v1/js-file` |
| Régénérer snapshot PHP | `POST /wp-json/sentin/v1/php-file` |
| Régénérer snapshot layouts | `POST /wp-json/sentin/v1/layouts-file` |
| Mettre à jour post_content | `POST /wp-json/sentin/v1/page-update` `{ id, content }` |
| Mettre à jour une option WP | `POST /wp-json/sentin/v1/option` `{ key, value }` (auth manage_options) |

### URLs snapshots (web_fetch — sans auth)
```
https://editionsdusentin.com/wp-content/uploads/sentin-css.txt
https://editionsdusentin.com/wp-content/uploads/sentin-js.txt
https://editionsdusentin.com/wp-content/uploads/sentin-php.txt
https://editionsdusentin.com/wp-content/uploads/sentin-layouts.txt
```

### Lire les modules code — méthode correcte

**Toujours utiliser `code-modules` — jamais le DOM.**

```javascript
// Lire tous les modules et_pb_code d'un layout ou page (contenu décodé)
var r = new XMLHttpRequest();
r.open('GET', '/wp-json/sentin/v1/code-modules?id=10620', false);
r.withCredentials = true;
r.setRequestHeader('X-WP-Nonce', window.wpApiSettings.nonce);
r.send();
var data = JSON.parse(r.responseText);
// data.count = nombre de modules
// data.modules[i].content = contenu HTML/JS/CSS décodé
// data.modules[i].attrs = attributs Divi du module
```

Le contenu retourné est **entièrement décodé** : `[et_pb_line_break_holder]` → `\n`, entités HTML résolues, `%91/%93` → `[/]`.

### Lire tous les layouts Theme Builder

```javascript
var r = new XMLHttpRequest();
r.open('GET', '/wp-json/sentin/v1/all-layouts', false);
r.withCredentials = true;
r.setRequestHeader('X-WP-Nonce', window.wpApiSettings.nonce);
r.send();
var layouts = JSON.parse(r.responseText).layouts;
// layouts[i] = { id, title, type, status, code_modules, content_decoded }
```

### IDs clés — Éditions du Sentin

| Ressource | ID |
|---|---|
| Page test accueil | 9334 |
| Body layout panier | 10620 |
| Page panier | 8 |
| Produits WC | UAH=2441, Bakchich=2452, Tapis rouge=2455, Hommes mariés=2456 |

---

## Règles CSS Divi — critiques

```
✅ top/right/bottom/left            ❌ inset
✅ display:flex + flex-wrap         ❌ display:grid / gap
✅ radial-gradient(circle at X% Y%) ❌ radial-gradient(ellipse at X% Y%)
✅ content:'' explicite             ❌ jamais auto-injecté
✅ filter:brightness() pour hover   ❌ transition entre gradients
```

- `%` dans calc() : `%%` en JSON import, `%` simples en shortcode live REST
- `background_blend="multiply"` sur fond sombre → invisible. Utiliser `soft-light`
- Apostrophes dans les noms de polices → parser Divi silencieux. Omettre les quotes
- `vertical_alignment="middle"` = attribut shortcode (pas CSS), requiert `make_equal="on"`
- `::after` sans `content:''` = effet nul
- Ne jamais sauvegarder via REST sans test visuel préalable

---

## Économie de contexte — obligatoire

- `zoom(region)` sans `save_to_disk` — jamais de screenshot plein écran
- JS (`getComputedStyle`) pour diagnostiquer, screenshot seulement pour valider
- Recharger la page si l'utilisateur a fait des modifications
- Retours JS filtrés — jamais de innerHTML complet ni CSS brut >300 chars
- `tool_search` une seule fois par session
- Une tâche = une conversation courte
