# modules-base.md — Modules de base Divi 4.27.6

---

## et_pb_text

```
[et_pb_text
  admin_label="Texte 1"
  _builder_version="4.27.6"
  _module_preset="default"
  text_font="Montserrat||on|on|||on|rgba(0,0,0,0.83)|"
  text_font_size="20px"
  text_letter_spacing="2px"
  text_line_height="1.4em"
  text_text_shadow_style="preset5"
  text_text_shadow_horizontal_length="0.09em"
  text_text_shadow_vertical_length="0.09em"
  text_text_shadow_blur_strength="0.01em"
  header_2_font="Cormorant Garamond||||||||"
  header_2_text_align="center"
  header_2_text_color="rgba(224,43,32,0.97)"
  header_2_font_size="21px"
  header_2_letter_spacing="0.05em"
  header_2_text_shadow_style="preset4"
  header_2_text_shadow_horizontal_length="0.01em"
  header_2_text_shadow_vertical_length="0.06em"
  header_2_text_shadow_blur_strength="0.01em"
  width="93%"
  max_width="96%"
  module_alignment="left"
  min_height="964px"
  max_height="840px"
  custom_margin="-13px|-16px|-14px|17px|false|false"
  custom_padding="4px|21px|17px|7px|false|false"
  link_option_url="https://..."
  global_colors_info="{}"]<p>Corps du texte</p>
<h2>Titre H2</h2>[/et_pb_text]
```

Préfixes typographie disponibles : `text_` `header_` `header_2_` à `header_6_`
⚠️ `text_orientation="center"` centre TOUT (titres + corps).

---

## et_pb_image

```
[et_pb_image
  src="https://editionsdusentin.com/wp-content/uploads/..."
  title_text="Texte alternatif"
  url="https://..."
  url_new_window="on"
  align="center"
  force_fullwidth="on"
  _builder_version="4.27.6"
  _module_preset="default"
  background_color="#E02B20"
  global_colors_info="{}"]
[/et_pb_image]
```

- PNG transparent : utiliser `filter: drop-shadow(...)` dans `custom_css_main_element`, pas `box-shadow`
- `force_fullwidth="on"` → l'image occupe 100% de la colonne

---

## et_pb_button

```
[et_pb_button
  button_url="https://..."
  button_text="Texte bouton"
  button_alignment="right"
  admin_label="Bouton"
  module_class="classe-css-bouton"
  _builder_version="4.27.6"
  _module_preset="default"
  custom_button="on"
  button_text_size="24px"
  button_text_color="#0C71C3"
  button_bg_use_color_gradient="on"
  button_bg_color_gradient_stops="#e02b20 8%|#000000 95%"
  button_border_width="11px"
  button_border_color="#EDF000"
  button_border_radius="5px"
  button_letter_spacing="6px"
  button_font="Montserrat|600||||on|||"
  button_use_icon="off"
  custom_margin="12px|5px|2px|5px|false|false"
  custom_padding="13px|8px|9px|16px|false|false"
  box_shadow_style="preset5"
  box_shadow_blur="4px"
  box_shadow_spread="-5px"
  button_text_color__hover_enabled="on|desktop"
  button_text_color__hover="#8300E9"
  button_bg_color__hover_enabled="on|desktop"
  button_bg_color_gradient_stops__hover="#e02b20 15%%|#000000 83%%"
  button_bg_use_color_gradient__hover="on"
  global_colors_info="{}"]
[/et_pb_button]
```

⚠️ `button_text_size` (pas `button_font_size`) — `button_bg_color` (pas `background_color`)
⚠️ `button_alignment` (pas `module_alignment`)
⚠️ Flèche native : `a.et_pb_button::after { display:none !important }`
⚠️ `custom_button="on"` obligatoire pour que les styles custom s'appliquent

---

## et_pb_code

```
[et_pb_code
  _builder_version="4.27.6"
  _module_preset="default"
  global_colors_info="{}"]<script>
</script>

<style>
</style>[/et_pb_code]
```

Sauts de ligne dans le contenu → `<!-- [et_pb_line_break_holder] -->` en JSON import.

---

## et_pb_team_member

```
[et_pb_team_member
  name="Nom"
  position="Sous-titre"
  image_url="https://..."
  _builder_version="4.27.6"
  _module_preset="default"
  header_font="Cormorant Garamond|700||||on|||"
  header_text_align="right"
  header_font_size="19px"
  header_letter_spacing="5px"
  header_line_height="1.1em"
  body_font="Montserrat||||||||"
  body_text_align="center"
  body_font_size="16px"
  body_letter_spacing="-2px"
  body_line_height="1.8em"
  position_font="Cormorant Garamond|300|||||||"
  position_text_align="center"
  position_font_size="17px"
  body_link_font="Montserrat||||||||"
  body_link_text_color="#E09900"
  text_orientation="left"
  border_radii_image="on|58px|58px|58px|58px"
  border_width_all_image="2px"
  box_shadow_style_image="preset2"
  global_colors_info="{}"]<p>Corps du texte</p>[/et_pb_team_member]
```

CSS ciblé : `custom_css_member_image` (image) — `custom_css_member_description` (description)

---

## et_pb_contact_form + et_pb_contact_field

```
[et_pb_contact_form
  captcha="off"
  email="adresse@exemple.com"
  title="Contactez-nous"
  success_message="Message reçu, merci !"
  submit_button_text="Envoyer"
  _builder_version="4.27.6"
  _module_preset="default"
  _unique_id="c977c16a-f3d8-47e5-ac44-6d23fe3660e7"
  form_field_background_color="#E02B20"
  form_field_text_color="#E09900"
  form_field_focus_background_color="#0C71C3"
  form_field_focus_text_color="#000000"
  form_field_custom_margin="10px|1em|10px|1em|true|true"
  form_field_custom_padding="1em|2em|1em|2em|true|true"
  form_field_font="Montserrat||||||||"
  form_field_letter_spacing="1px"
  form_field_line_height="1.6em"
  title_level="h2"
  title_font="Cormorant Garamond|300|||||||"
  title_font_size="25px"
  title_line_height="1.1em"
  background_color="#7CDA24"
  custom_button="on"
  button_text_size="26px"
  button_bg_color="#EDF000"
  button_use_icon="off"
  text_orientation="center"
  width="97%"
  module_alignment="right"
  global_colors_info="{}"]
  [et_pb_contact_field
    field_id="Name"
    field_title="Nom"
    _builder_version="4.16"
    global_colors_info="{}"]
  [/et_pb_contact_field]
  [et_pb_contact_field
    field_id="Email"
    field_title="Email"
    field_type="email"
    required_mark="on"
    _builder_version="4.16"
    global_colors_info="{}"]
  [/et_pb_contact_field]
[/et_pb_contact_form]
```

⚠️ `_unique_id` : générer un UUID v4 unique par formulaire
⚠️ Le bouton Submit n'accepte pas de `module_class` → cibler via `.et_pb_contact_form .et_pb_button`

---

## et_pb_fullwidth_header

⚠️ Utilisable uniquement dans une section `fullwidth="on"`.

```
[et_pb_fullwidth_header
  title="En-tête plein écran"
  subhead="Sous-titre"
  button_one_text="bouton 1"
  button_one_url="https://..."
  button_two_text="bouton 2"
  button_two_url="https://..."
  header_fullscreen="on"
  header_scroll_down="on"
  scroll_down_icon_color="#E02B20"
  scroll_down_icon_size="43px"
  _builder_version="4.27.6"
  _module_preset="default"
  title_level="h2"
  title_font="Cormorant Garamond||||||||"
  content_font_size="20px"
  custom_button_one="on"
  button_one_icon="&#x24;||divi||400"
  button_one_custom_padding="1em|2em|1em|2em|true|true"
  custom_button_two="on"
  button_two_bg_color="#EDF000"
  button_two_bg_enable_color="on"
  button_two_border_width="6px"
  button_two_border_color="#8e8e8e"
  button_two_font="|600||on|||||"
  button_two_use_icon="off"
  global_colors_info="{}"]<p>Contenu optionnel</p>[/et_pb_fullwidth_header]
```

⚠️ Rend un `<h1>` avant tout le contenu. Si l'ordre visuel diffère : `title=""` + `display:none` en CSS.
⚠️ `button_one_icon` : format `"code||fontset||poids"` — ex. `"&#x24;||divi||400"`
