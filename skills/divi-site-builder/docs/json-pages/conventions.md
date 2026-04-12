# conventions.md — Syntaxe globale Divi 4.27.6

Source : JSON de référence exportés depuis Divi 4.27.6 (pages standard).

---

## Attributs obligatoires sur chaque shortcode

```
_builder_version="4.27.6"
_module_preset="default"
global_colors_info="{}"
```

Sur sections et modules clés : `module_id="id-css"` `module_class="classe-css"`

---

## Syntaxe font

Format : `"Famille|poids|italique|souligné|surligne|barré|majuscules|couleur|espacement"`

Les positions vides restent vides (jamais omises) :
```
text_font="Montserrat||on|on|||on|rgba(0,0,0,0.83)|"   ← 9 segments, dernier vide
header_2_font="Cormorant Garamond||||||||"               ← tout défaut
button_font="Montserrat|600||||on|||"                    ← poids 600, majuscules pos 6
table_header_font="Assistant|700||on|||||"               ← poids 700, souligné pos 4
```

Poids valides : `100` `200` `300` `400` `500` `600` `700` `800` `900`
Pas d'apostrophes autour du nom de police — le parser Divi devient silencieux.

---

## Syntaxe border_radii

Format : `"on|top-left|top-right|bottom-right|bottom-left"`
```
border_radii="on|30px|30px|30px|30px"
border_radii_image="on|6px|6px|6px|6px"
border_radii_image="on|58px|58px|58px|58px"   ← image ronde
```

Borders par côté (priorité sur `border_width_all`) :
```
border_width_top="6px"    border_color_top="#7CDA24"
border_width_right="3px"  border_color_right="#0C71C3"
border_width_bottom="2px" border_color_bottom="rgba(224,43,32,0.39)"
border_width_left="2px"   border_color_left="#EDF000"
```

---

## Syntaxe gradients

Fond dégradé linéaire :
```
use_background_color_gradient="on"
background_color_gradient_stops="#e02b20 0%|#000000 95%"
```

Fond dégradé avec image par-dessus :
```
use_background_color_gradient="on"
background_color_gradient_stops="rgba(224,153,0,0.73) 0%|rgba(41,196,169,0.78) 100%"
background_color_gradient_overlays_image="on"
background_image="https://..."
```

Gradient bouton :
```
button_bg_use_color_gradient="on"
button_bg_color_gradient_stops="#e02b20 8%|#000000 95%"
```

Gradient circulaire :
```
button_bg_color_gradient_type="circular"
button_bg_color_gradient_stops="#e02b20 0%|#e09900 100%"
```

Gradient elliptique :
```
button_bg_color_gradient_type="elliptical"
```

Séparateur de stops : `|` (pipe). Dans CSS embarqué : doubler les `%%`.

---

## Syntaxe hover

```
button_text_color__hover_enabled="on|desktop"
button_text_color__hover="#8300E9"
button_bg_color__hover_enabled="on|desktop"
button_bg_color_gradient_stops__hover="#e02b20 15%|#000000 83%%"
button_bg_use_color_gradient__hover="on"

icon_background_color__hover_enabled="on|desktop"
icon_background_color__hover="#FFFFFF"
icon_text_color__hover_enabled="on|hover"
icon_text_color__hover="#E02B20"

fields_focus_text_color__hover_enabled="on|hover"
fields_focus_text_color__hover="#8300E9"
```

Pattern : `[attribut]__hover_enabled="on|desktop"` puis `[attribut]__hover="valeur"`

---

## Syntaxe spacing (margin / padding)

Format : `"top|right|bottom|left|linked|linked-vertical"`
```
custom_margin="20px|15px|20px|15px|false|true"
custom_padding="20px|15px|35px|10px|false|false"
```
- `true` = valeurs liées (modification simultanée)
- `false` = valeurs indépendantes
- Valeur vide = non définie : `"5px||7px||false|false"`

---

## Syntaxe box_shadow

```
box_shadow_style="preset1"    ← ombre portée standard
box_shadow_style="preset2"    ← ombre intérieure
box_shadow_style="preset5"    ← ombre diffuse
box_shadow_vertical="8px"
box_shadow_blur="21px"
box_shadow_spread="1px"
```

---

## Syntaxe filtres CSS

```
filter_hue_rotate="18deg"
filter_saturate="90%"
filter_brightness="86%"
filter_invert="7%"
filter_sepia="14%"
filter_opacity="92%"
filter_blur="2px"
```

---

## Syntaxe transform

```
transform_scale="103%|103%"
transform_translate="7px|5px"
transform_translate_linked="off"
transform_rotate="33deg|9deg|5deg"
```

---

## Syntaxe divider (séparateur de section)

```
top_divider_style="curve"
top_divider_color="#E02B20"
top_divider_height="83px"
top_divider_repeat="5x"
top_divider_flip="horizontal|vertical"
```

---

## Syntaxe animation (sur colonnes et modules)

```
animation_style="fade"
animation_duration="1100ms"
animation_delay="250ms"
```

---

## Syntaxe custom_css (champs CSS Divi)

```
custom_css_before="content: %22%22"         ← content:""  (%22 = guillemet encodé)
custom_css_main_element="content: %22%22"
custom_css_after="content: %22%22"
custom_css_free_form=".classe-css {||  display: block;||}"
```

Sauts de ligne → `||`. `%` dans valeurs CSS → `%%`.

---

## Transitions hover

```
hover_transition_duration="320ms"
hover_transition_delay="365ms"
```

---

## hover_enabled / sticky_enabled

Sur les pages standard, ces attributs apparaissent sur les modules qui ont des états hover ou sticky configurés :
```
hover_enabled="0"
sticky_enabled="0"
```
Valeur `"0"` = désactivé. Omettre si non utilisé.
