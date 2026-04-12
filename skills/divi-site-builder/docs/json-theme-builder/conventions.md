# json-theme-builder/conventions.md — Différences vs pages standard

Lire d'abord `docs/json-pages/conventions.md` et les fichiers modules.
Ce fichier documente uniquement ce qui **diffère** en contexte Theme Builder.

---

## Différence 1 : `theme_builder_area="post_content"`

Attribut à ajouter sur **chaque shortcode** sans exception :
section, row, column, et tous les modules.

```
[et_pb_section fb_built="1" fullwidth="on"
  admin_label="Section pleine largeur"
  _builder_version="4.27.6"
  _module_preset="default"
  global_colors_info="{}"
  theme_builder_area="post_content"]

  [et_pb_fullwidth_header
    title="En-tête plein écran"
    _builder_version="4.27.6"
    _module_preset="default"
    global_colors_info="{}"
    theme_builder_area="post_content"]...[/et_pb_fullwidth_header]

[/et_pb_section]
```

---

## Différence 2 : ordre des attributs

Dans le Theme Builder, les attributs de **contenu** (`title=`, `subhead=`, `name=`, `product=`, etc.) sont placés **avant** `_builder_version`.

Page standard :
```
[et_pb_fullwidth_header _builder_version="4.27.6" ... title="Titre" ...]
```

Theme Builder :
```
[et_pb_fullwidth_header title="Titre" subhead="Sous-titre" ... _builder_version="4.27.6" ...]
```

Divi accepte les deux ordres — mais reproduire la convention Theme Builder évite les surprises à l'import.

---

## Différence 3 : `hover_enabled` et `sticky_enabled` absents

Sur les pages standard, ces attributs apparaissent explicitement :
```
hover_enabled="0"
sticky_enabled="0"
```

Dans le Theme Builder, ils sont **omis** sur la quasi-totalité des modules.
Ne pas les inclure sauf si un état hover ou sticky est réellement configuré.

---

## Différence 4 : `fb_built="1"` sur les sections

Présent sur les sections dans les deux contextes. Ne pas omettre.

---

## Récapitulatif — checklist Theme Builder

- [ ] `theme_builder_area="post_content"` sur chaque shortcode
- [ ] Attributs de contenu avant `_builder_version`
- [ ] `hover_enabled` / `sticky_enabled` omis (sauf si utilisés)
- [ ] `fb_built="1"` sur chaque `et_pb_section`
- [ ] Toutes les autres règles de `json-pages/conventions.md` s'appliquent
