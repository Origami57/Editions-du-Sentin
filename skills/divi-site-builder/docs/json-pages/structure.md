# structure.md — Section, Row, Column (Divi 4.27.6)

---

## et_pb_section

### Section standard (fond uni + dégradé + image)
```
[et_pb_section fb_built="1"
  admin_label="Première section"
  module_id="id-css"
  module_class="classe-css"
  _builder_version="4.27.6"
  _module_preset="default"
  background_color="rgba(255,255,255,0.51)"
  use_background_color_gradient="on"
  background_color_gradient_stops="rgba(224,153,0,0.73) 0%|rgba(41,196,169,0.78) 100%"
  background_color_gradient_overlays_image="on"
  background_image="https://..."
  z_index="1"
  width="93%"
  max_width="1920px"
  min_height="311px"
  height="362px"
  max_height="383px"
  overflow-x="hidden"
  custom_margin="20px|15px|20px|15px|false|true"
  custom_padding="20px|15px|35px|10px|false|false"
  border_radii="on|30px|30px|30px|30px"
  border_width_all="3px"
  border_color_all="#E02B20"
  box_shadow_style="preset1"
  box_shadow_vertical="8px"
  box_shadow_blur="21px"
  box_shadow_spread="1px"
  filter_hue_rotate="18deg"
  filter_saturate="90%"
  filter_brightness="86%"
  filter_blur="2px"
  top_divider_style="curve"
  top_divider_color="#E02B20"
  top_divider_height="83px"
  top_divider_flip="horizontal|vertical"
  transform_scale="103%|103%"
  transform_rotate="33deg|9deg|5deg"
  hover_transition_duration="320ms"
  hover_transition_delay="365ms"
  custom_css_before="content: %22%22"
  custom_css_main_element="content: %22%22"
  custom_css_after="content: %22%22"
  custom_css_free_form=".classe-css {||  display: block;||}"
  global_colors_info="{}"]
```

### Section pleine largeur (fullwidth)
```
[et_pb_section fb_built="1"
  fullwidth="on"
  admin_label="Section pleine largeur"
  _builder_version="4.27.6"
  _module_preset="default"
  global_colors_info="{}"]
```

⚠️ Une section `fullwidth="on"` n'accepte que des modules fullwidth (`et_pb_fullwidth_header`, `et_pb_fullwidth_image`, etc.) — pas de `et_pb_row`.

---

## et_pb_row

### Attributs communs
```
_builder_version="4.27.6"
_module_preset="default"
global_colors_info="{}"
```

Attributs optionnels :
```
use_custom_gutter="on"  gutter_width="2"
make_equal="on"          ← requis pour vertical_alignment="middle" sur colonnes
width="90%"
max_width="1200px"
custom_margin="..."
custom_padding="..."
```

### Toutes les structures de colonnes disponibles

```
column_structure="4_4"              ← 1 colonne pleine largeur (défaut si omis)
column_structure="1_2,1_2"          ← 2 colonnes égales
column_structure="1_3,1_3,1_3"      ← 3 colonnes égales
column_structure="1_4,1_4,1_4,1_4" ← 4 colonnes égales
column_structure="2_5,3_5"
column_structure="3_5,2_5"
column_structure="1_3,2_3"
column_structure="2_3,1_3"
column_structure="1_4,3_4"
column_structure="3_4,1_4"
column_structure="1_6,1_6,1_6,1_2"
```

### Exemple row 2 colonnes
```
[et_pb_row column_structure="1_2,1_2"
  _builder_version="4.27.6"
  _module_preset="default"
  global_colors_info="{}"]
  [et_pb_column type="1_2" ...][/et_pb_column]
  [et_pb_column type="1_2" ...][/et_pb_column]
[/et_pb_row]
```

---

## et_pb_column

Le `type` doit correspondre à la position dans `column_structure` du row parent.

```
[et_pb_column type="4_4"
  _builder_version="4.27.6"
  _module_preset="default"
  custom_padding="20px|2px|15px|2px|false|false"
  animation_style="fade"
  animation_duration="1100ms"
  animation_delay="250ms"
  border_radii="on|6px|6px|6px|6px"
  border_width_top="6px"    border_color_top="#7CDA24"
  border_width_right="3px"  border_color_right="#0C71C3"
  border_width_bottom="2px" border_color_bottom="rgba(224,43,32,0.39)"
  border_width_left="2px"   border_color_left="#EDF000"
  box_shadow_style="preset2"
  global_colors_info="{}"]
```

Types valides : `4_4` `1_2` `1_3` `2_3` `1_4` `3_4` `1_5` `2_5` `3_5` `4_5` `1_6`
