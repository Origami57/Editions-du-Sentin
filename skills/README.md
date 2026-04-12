# Claude Skills — Éditions du Sentin

Bibliothèque de skills pour Claude (claude.ai et Claude Code).  
Chaque skill est un dossier autonome contenant un `SKILL.md` à lire en premier.

---

## Sommaire des skills

| Skill | Taille | Description courte |
|---|---|---|
| [`divi-site-builder/`](./divi-site-builder/SKILL.md) | ~65 KB | Création et édition de sites Divi (JSON, live editing, CSS debug) |
| [`frontend-design/`](./frontend-design/SKILL.md) | ~4.5 KB | Interfaces frontend production-grade, haute qualité design |
| [`web-dev-diagnostic/`](./web-dev-diagnostic/SKILL.md) | ~7 KB | Diagnostic CSS/JS/WordPress avant tout code — max 3 tentatives |

---

## Comment utiliser un skill

1. **Lire le `SKILL.md`** du skill concerné — il contient le workflow, les règles et les pointeurs vers les fichiers de référence.
2. **Suivre les instructions à la lettre** — chaque skill encode une méthodologie issue d'itérations réelles.
3. **Ne pas improviser** : si une règle semble restrictive, c'est intentionnel.

---

## Structure type d'un skill

```
skill-name/
├── SKILL.md          ← Point d'entrée obligatoire
└── docs/             ← Fichiers de référence (lus sur instruction du SKILL.md)
    ├── *.md
    └── sous-dossiers/
```

---

## Skill : divi-site-builder

Trois modes de travail :
- **Mode JSON** : génération de JSON importable dans Divi depuis un prototype HTML
- **Mode Live** : édition directe via Chrome MCP + REST API WordPress
- **Mode Debug** : surcharge CSS, diagnostic de rendu

Voir [`divi-site-builder/SKILL.md`](./divi-site-builder/SKILL.md) puis les docs dans [`divi-site-builder/docs/`](./divi-site-builder/docs/).

---

## Skill : frontend-design

Création d'interfaces web distinctives, production-grade.  
Voir [`frontend-design/SKILL.md`](./frontend-design/SKILL.md).

---

## Skill : web-dev-diagnostic

Méthodologie diagnostic-first pour tout travail CSS/JS/WordPress/WooCommerce.  
**Règle clé** : Phase 0 obligatoire (lire le code fourni) → max 3 tentatives par approche.  
Voir [`web-dev-diagnostic/SKILL.md`](./web-dev-diagnostic/SKILL.md).
