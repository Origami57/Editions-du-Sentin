# json-generation.md — Prototype HTML → JSON Divi

## Phase 1 : Prototype HTML

### Règles
- Fichier : `create_file` → `/mnt/user-data/outputs/prototype-[page].html`
- Modifications : `str_replace` uniquement (régénérer complet si >50% changé)
- Images : jamais base64 — URLs WordPress, GitHub raw, ou `placehold.co`
- Google Fonts ne fonctionne pas dans wkhtmltoimage → fallbacks système

Annotations obligatoires sur chaque section :
```html
<!-- SECTION: Hero | fullwidth="on" | bg:#1a0900 | module_class="prefix-hero" -->
<!-- MODULE: et_pb_fullwidth_header | title, subhead, 2 boutons -->
```

Validation : `html-screenshot` → `view` → corriger → `present_files`
**Attendre validation explicite avant Phase 2.**

---

### Gabarit HTML — simuler le comportement Divi

**OBLIGATOIRE** : tout prototype HTML doit utiliser ce gabarit comme base. Il reproduit
fidèlement les wrappers, marges, paddings et comportements par défaut de Divi 4.x,
de sorte que le rendu prototype soit prédictif du rendu Divi réel.

```html
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Prototype</title>
<style>

/* ============================================================
   RESET + BASE DIVI
   ============================================================ */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: Georgia, "Times New Roman", serif; /* police par défaut Divi */
  font-size: 14px;
  line-height: 1.7em;
  color: #666;
  background: #fff;
  -webkit-font-smoothing: antialiased;
}

/* ============================================================
   WRAPPERS DIVI
   Hiérarchie : #page-container > .et_pb_section > .et_pb_row
                > .et_pb_column > [module]
   ============================================================ */

#page-container {
  width: 100%;
  overflow: hidden;
}

/* et_pb_section — padding vertical par défaut Divi */
.et_pb_section {
  position: relative;
  width: 100%;
  padding: 54px 0;       /* défaut Divi ≈ 4% sur viewport 1350px */
  background-color: transparent;
}

/* Section fullwidth — pas de padding, contient des modules fullwidth, pas de row */
.et_pb_section.fullwidth {
  padding: 0;
}

/* et_pb_row — centré, max-width 1080px, padding horizontal par défaut */
.et_pb_row {
  width: 80%;
  max-width: 1080px;
  margin: 0 auto;
  padding: 2% 0;
  display: flex;
  flex-wrap: wrap;
  align-items: flex-start;
}

/* et_pb_column — gouttière par défaut */
.et_pb_column {
  position: relative;
  padding: 0 15px;
}

/* ============================================================
   STRUCTURES DE COLONNES — column_structure Divi
   ============================================================ */
.col-4_4  { width: 100%; }
.col-1_2  { width: 50%; }
.col-1_3  { width: 33.333%; }
.col-2_3  { width: 66.666%; }
.col-1_4  { width: 25%; }
.col-3_4  { width: 75%; }
.col-1_5  { width: 20%; }
.col-2_5  { width: 40%; }
.col-3_5  { width: 60%; }
.col-4_5  { width: 80%; }
.col-1_6  { width: 16.666%; }

/* ============================================================
   MODULES — comportements par défaut Divi
   ============================================================ */

/* et_pb_text */
.et_pb_text { word-wrap: break-word; position: relative; }
.et_pb_text p { padding-bottom: 1em; }
.et_pb_text h1, .et_pb_text h2, .et_pb_text h3,
.et_pb_text h4, .et_pb_text h5, .et_pb_text h6 {
  line-height: 1.4em;
  padding-bottom: 10px;
  color: #333;
}

/* et_pb_image */
.et_pb_image { text-align: center; margin: 0 auto; }
.et_pb_image img { max-width: 100%; height: auto; display: inline-block; }
.et_pb_image.force_fullwidth img { width: 100%; }

/* et_pb_button */
.et_pb_button_wrapper { display: block; padding: 10px 0; }
.et_pb_button_wrapper.align-right  { text-align: right; }
.et_pb_button_wrapper.align-center { text-align: center; }
.et_pb_button_wrapper.align-left   { text-align: left; }
.et_pb_button {
  display: inline-block;
  font-size: 20px;
  font-weight: 500;
  padding: 0.3em 1em;
  letter-spacing: 2px;
  border-radius: 3px;
  border: 2px solid currentColor;
  background: transparent;
  color: #2ea3f2;
  cursor: pointer;
  text-decoration: none;
  position: relative;
}
/* Flèche native Divi — masquer si button_use_icon="off" */
.et_pb_button::after {
  content: "\32";
  font-family: "ETmodules";
  margin-left: 0.5em;
  font-size: 16px;
}
.et_pb_button.no-icon::after { display: none; }

/* et_pb_fullwidth_header */
.et_pb_fullwidth_header {
  width: 100%;
  min-height: 100vh;      /* header_fullscreen="on" */
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  padding: 60px 30px;
  position: relative;
}
.et_pb_fullwidth_header .et_pb_title_container { max-width: 900px; }
.et_pb_fullwidth_header h2 { font-size: 60px; line-height: 1.2em; margin-bottom: 20px; }
.et_pb_fullwidth_header .et_pb_header_subhead { font-size: 24px; margin-bottom: 30px; }
.et_pb_fullwidth_header .et_pb_button_wrapper { display: inline-block; margin: 0 8px; }

/* et_pb_team_member */
.et_pb_team_member { text-align: center; }
.et_pb_team_member img {
  width: 90px; height: 90px;
  border-radius: 50%;
  display: block;
  margin: 0 auto 15px;
  object-fit: cover;
}
.et_pb_team_member h4 { font-size: 20px; margin-bottom: 4px; }
.et_pb_team_member .et_pb_member_position { font-size: 14px; color: #999; margin-bottom: 10px; }

/* et_pb_contact_form */
.et_pb_contact_form_container { padding: 20px; }
.et_pb_contact_form_container input,
.et_pb_contact_form_container textarea {
  width: 100%; padding: 16px; margin-bottom: 10px;
  border: 1px solid #ddd; font-size: 14px; display: block;
}
.et_pb_contact_form_container textarea { min-height: 120px; }

/* ============================================================
   UTILITAIRES
   ============================================================ */
.text-center { text-align: center; }
.text-left   { text-align: left; }
.text-right  { text-align: right; }

/* ============================================================
   STYLES SPÉCIFIQUES AU PROTOTYPE — ajouter ici
   ============================================================ */

</style>
</head>
<body>
<div id="page-container">

  <!-- SECTION: standard | bg:#fff | module_class="prefix-section" -->
  <div class="et_pb_section">
    <div class="et_pb_row">

      <!-- column_structure="1_2,1_2" -->
      <div class="et_pb_column col-1_2">
        <!-- MODULE: et_pb_text -->
        <div class="et_pb_text">
          <h2>Titre</h2>
          <p>Corps du texte.</p>
        </div>
      </div>

      <div class="et_pb_column col-1_2">
        <!-- MODULE: et_pb_image -->
        <div class="et_pb_image">
          <img src="https://placehold.co/600x400" alt="Image">
        </div>
      </div>

    </div>
  </div>

  <!-- SECTION: fullwidth="on" | module_class="prefix-hero" -->
  <div class="et_pb_section fullwidth" style="background:#1a0900;">
    <!-- MODULE: et_pb_fullwidth_header -->
    <div class="et_pb_fullwidth_header" style="color:#fff;">
      <div class="et_pb_title_container">
        <h2 style="color:#fff;">Titre hero</h2>
        <p class="et_pb_header_subhead">Sous-titre</p>
        <div>
          <span class="et_pb_button_wrapper align-center" style="display:inline-block; margin:0 8px;">
            <a class="et_pb_button no-icon" style="color:#fff; border-color:#fff;">Bouton 1</a>
          </span>
          <span class="et_pb_button_wrapper align-center" style="display:inline-block; margin:0 8px;">
            <a class="et_pb_button no-icon" style="background:#EDF000; color:#000; border-color:#EDF000;">Bouton 2</a>
          </span>
        </div>
      </div>
    </div>
  </div>

</div><!-- /#page-container -->
</body>
</html>
```

### Règles d'utilisation du gabarit

1. **Ne jamais s'écarter de la hiérarchie** `.et_pb_section > .et_pb_row > .et_pb_column > [module]`
2. **Toujours utiliser les classes de colonnes** (`col-1_2`, `col-1_3`, etc.) — pas de width CSS libre sur les colonnes
3. **Ne pas neutraliser** les paddings/marges du gabarit sauf si le design le demande explicitement — noter dans l'annotation pour le JSON
4. **Styles visuels** (couleurs, polices, bordures, ombres) → dans la zone `STYLES SPÉCIFIQUES AU PROTOTYPE` ou en `style=""` inline, jamais en modifiant les règles du gabarit
5. **Polices** : utiliser uniquement des polices système ou chargées via `@font-face` depuis une URL WordPress — pas `fonts.googleapis.com` (incompatible wkhtmltoimage)
6. **Viewport de référence** : 1280px — largeur cible pour screenshots et rendu Divi desktop

---

## Phase 2 : JSON Divi

### Structure enveloppe
```json
{
  "context": "et_builder",
  "data": {
    "1": "[et_pb_section ...][et_pb_row ...][et_pb_column ...][et_pb_text ...]...[/et_pb_text][/et_pb_column][/et_pb_row][/et_pb_section]"
  }
}
```
- `data["1"]` = chaîne de shortcodes (pas objet imbriqué)
- Guillemets dans la chaîne : échappés `\"`
- `_builder_version="4.27.6"` sur chaque shortcode
- `locked="off"` `global_colors_info="{}"` sur chaque module
- `module_class="prefix-nom"` sur sections et modules clés

### Référence syntaxique — selon le contexte

| Contexte | Référence à lire |
|---|---|
| Page standard (éditeur Divi classique) | `docs/json-pages/conventions.md` + fichiers modules |
| Template Theme Builder | `docs/json-theme-builder/conventions.md` (delta uniquement) |

**Toujours lire `docs/json-pages/conventions.md` en premier**, même pour le Theme Builder.

---

### CSS dans le JSON (≠ shortcode live)
→ Règles CSS acceptées/rejetées par Divi : voir **SKILL.md § CSS — règles critiques**.

`%` dans calc() et gradients → **doubler en `%%`** dans les JSON d'import :
```
background: radial-gradient(circle at 35%% 45%%, rgba(0,0,0,0) 30%%, rgba(0,0,0,.7) 100%%)
```
Sauts de ligne → encodés `||` :
```
.ma-classe {||  propriete: valeur;||}
```
Modification via Python : travailler en **bytes bruts** (évite bugs encodage `||`).

---

### Modules — pièges spécifiques

**et_pb_fullwidth_header**
- Rend `<h1>` avant tout le reste → si ordre visuel différent : `title=""` vide + `display:none` en CSS
- `button_one_text` `button_one_url` `button_two_text` `button_two_url`

**et_pb_button**
- `button_text_size` (pas `button_font_size`)
- `button_bg_color` (pas `background_color`)
- `button_alignment` (pas `module_alignment`)
- Flèche native persistante → CSS : `a.et_pb_button::after { display:none !important }`
- Boutons côte à côte : `module_class="inline-btn"` + CSS global `.inline-btn .et_pb_button_module_wrapper { display:inline-block !important }`

**et_pb_image**
- PNG transparent : `filter: drop-shadow(...)` dans `custom_css_main_element` — pas `box-shadow`

**et_pb_shop**
- Overlay hover désactivé : `icon_hover_color="rgba(255,255,255,0)"` `hover_overlay_color="rgba(255,255,255,0)"`
- Masquer produit spécifique : `.et_pb_shop_0 .post-[ID] { display:none !important }` dans `custom_css_free_form`
- Espace sous images : `font-size:0; line-height:0` sur le conteneur produit

**et_pb_text**
- `text_orientation="center"` centre tout (titre + corps) — voir SKILL.md pour arabesque centrée + corps gauche

**et_pb_wc_add_to_cart**
- WooCommerce surcharge `transition: opacity !important` → sélecteur très spécifique requis pour override
- Background sur wrapper >> bouton → cibler `.button` directement

**et_pb_contact_form**
- Bouton Submit ne reçoit pas de `module_class` → cibler via sélecteur Divi généré

**et_pb_team_member**
- `custom_css_member_image` — CSS sur l'image (border, width…)
- `custom_css_member_description` — CSS sur la description

---

### CSS override Divi — spécificité

Divi génère des règles inline très spécifiques :
```css
body #page-container .et_pb_section .et_pb_text_3 .element { ... !important }
```
Pour écraser : même niveau de spécificité ou ajouter `!important` avec sélecteur suffisant.

Lire les règles calculées avant tout override :
```javascript
Array.from(document.styleSheets).forEach(s => {
  try { Array.from(s.cssRules).forEach(r => {
    if (r.selectorText?.includes('MOT_CLE'))
      console.log(r.selectorText, r.style.cssText);
  })} catch(e){}
});
```

---

### Checklist avant livraison JSON

- [ ] `"context": "et_builder"` présent
- [ ] Pas d'apostrophes autour des noms de polices
- [ ] Pas de `display:grid` / `gap` / `inset`
- [ ] `%%` pour les `%` dans le CSS
- [ ] `content:''` dans tous les `::before`/`::after`
- [ ] Balises équilibrées (section/row/column/module)
- [ ] `_builder_version` sur chaque shortcode
- [ ] `theme_builder_area="post_content"` si Template Theme Builder
