# modules-woo.md — Modules WooCommerce Divi 4.27.6

---

## et_pb_shop (catalogue produits)

```
[et_pb_shop
  posts_number="4"
  orderby="date-desc"
  show_rating="off"
  show_sale_badge="off"
  icon_hover_color="rgba(255,255,255,0)"
  hover_overlay_color="rgba(193,193,193,0)"
  _builder_version="4.27.6"
  _module_preset="default"
  title_text_align="center"
  title_font_size="18px"
  border_radii_image="on|3px|3px|3px|3px"
  border_width_all_image="2px"
  global_colors_info="{}"]
[/et_pb_shop]
```

`orderby` valeurs : `date-desc` `date-asc` `price-desc` `price-asc` `sales` `rating` `menu_order`
Overlay désactivé : `icon_hover_color="rgba(255,255,255,0)"` + `hover_overlay_color="rgba(255,255,255,0)"`

---

## et_pb_wc_images

```
[et_pb_wc_images
  product="1605"
  show_product_gallery="off"
  show_sale_badge="off"
  _builder_version="4.27.6"
  _module_preset="default"
  background_color="#E09900"
  global_colors_info="{}"]
[/et_pb_wc_images]
```

`product` : ID WordPress du produit (ou `current` pour la page produit courante)

---

## et_pb_wc_title

```
[et_pb_wc_title
  _builder_version="4.27.6"
  _module_preset="default"
  header_level="h2"
  header_font="|||on|||||"
  header_font_size="25px"
  header_letter_spacing="1px"
  header_line_height="0.9em"
  header_text_shadow_style="preset2"
  global_colors_info="{}"]
[/et_pb_wc_title]
```

---

## et_pb_wc_price

```
[et_pb_wc_price
  product="2456"
  _builder_version="4.27.6"
  _module_preset="default"
  body_font="Cormorant Garamond||||||||"
  body_font_size="24px"
  text_orientation="left"
  global_colors_info="{}"]
[/et_pb_wc_price]
```

---

## et_pb_wc_meta

```
[et_pb_wc_meta
  product="2452"
  show_sku="off"
  _builder_version="4.27.6"
  _module_preset="default"
  body_font="|||on|||||"
  body_font_size="16px"
  global_colors_info="{}"]
[/et_pb_wc_meta]
```

---

## et_pb_wc_add_to_cart

```
[et_pb_wc_add_to_cart
  product="2452"
  show_quantity="off"
  show_stock="off"
  _builder_version="4.27.6"
  _module_preset="default"
  fields_background_color="#E09900"
  fields_text_color="#000000"
  fields_focus_background_color="#E09900"
  fields_focus_text_color="#0C71C3"
  fields_focus_text_color__hover_enabled="on|hover"
  fields_focus_text_color__hover="#8300E9"
  custom_button="on"
  button_text_size="22px"
  button_text_color="#FFFFFF"
  button_bg_use_color_gradient="on"
  button_bg_color_gradient_type="circular"
  button_bg_color_gradient_stops="#e02b20 0%%|#e09900 100%%"
  button_border_color="#0C71C3"
  button_use_icon="off"
  global_colors_info="{}"]
[/et_pb_wc_add_to_cart]
```

⚠️ WooCommerce surcharge `transition: opacity !important` → sélecteur très spécifique requis
⚠️ Background sur wrapper > bouton → cibler `.button` directement

---

## et_pb_wc_upsells

```
[et_pb_wc_upsells
  posts_number="4"
  columns_number="4"
  show_rating="off"
  show_sale_badge="off"
  icon_hover_color="rgba(124,218,36,0)"
  hover_overlay_color="rgba(198,198,198,0)"
  _builder_version="4.27.6"
  _module_preset="default"
  title_font="|||||on|||"
  title_font_size="24px"
  global_colors_info="{}"]
[/et_pb_wc_upsells]
```

---

## et_pb_wc_cross_sells

```
[et_pb_wc_cross_sells
  _builder_version="4.27.6"
  _module_preset="default"
  title_text_color="#E02B20"
  price_font="Montserrat||||||||"
  price_font_size="13px"
  price_text_shadow_style="preset3"
  global_colors_info="{}"]
[/et_pb_wc_cross_sells]
```

---

## et_pb_wc_cart_totals

```
[et_pb_wc_cart_totals
  collapse_table_gutters_borders="on"
  placeholder_color="#000000"
  _builder_version="4.27.6"
  _module_preset="default"
  background_color="#FFFFFF"
  title_font="Montserrat|600|on||||||"
  title_text_align="right"
  title_text_color="#E02B20"
  title_font_size="24px"
  title_letter_spacing="-1px"
  title_line_height="1.1em"
  column_label_font="Montserrat|700|||||||"
  column_label_text_align="left"
  column_label_text_color="#000000"
  column_label_font_size="13px"
  column_label_letter_spacing="1px"
  column_label_line_height="1.4em"
  body_font="Montserrat||||||||"
  body_text_color="#E09900"
  link_text_color="#E02B20"
  table_custom_margin="-1px|3px|2px|3px|false|true"
  table_cell_background_color="#E02B20"
  table_cell_custom_padding="1px|3px|1px|3px|true|true"
  border_width_bottom_table_row="2px"
  form_field_background_color="#FFFFFF"
  form_field_text_color="#0C71C3"
  form_field_focus_background_color="#8300E9"
  form_field_focus_text_color="#000000"
  form_field_custom_margin="2px|-19px|2px|-13px|true|false"
  form_field_custom_padding="2px|0px|2px|0px|true|true"
  form_field_font="Assistant||||||||"
  custom_button="on"
  button_text_size="18px"
  button_text_color="#E02B20"
  button_bg_color="#000000"
  button_bg_enable_color="on"
  button_border_width="3px"
  button_border_color="#EDF000"
  button_font="Montserrat||||||||"
  button_use_icon="off"
  custom_margin="7px|4px|3px|9px|false|false"
  custom_padding="8px|6px|8px|9px|false|false"
  border_radii="on|3px|3px|3px|3px"
  border_width_all="2px"
  global_colors_info="{}"]
[/et_pb_wc_cart_totals]
```

---

## et_pb_wc_cart_products

```
[et_pb_wc_cart_products
  vertical_gutter_width="2px"
  horizontal_gutter_width="2px"
  placeholder_color="#000000"
  show_coupon_code="off"
  _builder_version="4.27.6"
  _module_preset="default"
  background_color="#E02B20"
  table_header_font="Assistant|700||on|||||"
  table_header_letter_spacing="2px"
  body_font="Assistant||||||||"
  body_text_color="#000000"
  link_font="Assistant||||||||"
  link_text_color="#E02B20"
  table_custom_margin="4px|2px|6px|2px|false|false"
  table_custom_padding="8px|9px|9px|6px|false|false"
  table_row_background_color="#E09900"
  table_cell_background_color="#E02B20"
  table_cell_alternating_background_color="#E09900"
  border_width_bottom_table="2px"
  border_color_bottom_table="#000000"
  border_radii_image="on|2px|2px|2px|2px"
  icon_background_color="#E02B20"
  icon_text_color="#FFFFFF"
  icon_font_size="1.7em"
  icon_background_color__hover_enabled="on|desktop"
  icon_background_color__hover="#FFFFFF"
  icon_text_color__hover_enabled="on|hover"
  icon_text_color__hover="#E02B20"
  form_field_background_color="#FFFFFF"
  form_field_text_color="rgba(0,0,0,0.87)"
  form_field_focus_background_color="#000000"
  form_field_focus_text_color="#E02B20"
  form_field_custom_margin="3px|-8px|9px|-8px|false|true"
  form_field_custom_padding="0px|0px|0px|0px|false|false"
  form_field_font="Cormorant Garamond||||||||"
  custom_button="on"
  button_text_size="22px"
  button_text_color="#E02B20"
  button_bg_color="#000000"
  button_bg_enable_color="on"
  button_font="Cormorant Garamond||||||||"
  button_use_icon="off"
  button_custom_margin="12px||||false|false"
  button_custom_padding="10px|7px|5px|7px|false|true"
  custom_disabled_button="on"
  disabled_button_text_color="#FFFFFF"
  disabled_button_bg_color="#707070"
  disabled_button_bg_enable_color="on"
  disabled_button_use_icon="off"
  custom_margin="3px|6px|3px|6px|true|true"
  custom_padding="17px|2px|3px|5px|false|false"
  global_colors_info="{}"]
[/et_pb_wc_cart_products]
```

---

## et_pb_wc_cart_notice

Trois variantes selon `page_type` :

```
[et_pb_wc_cart_notice
  _builder_version="4.27.6"
  _module_preset="default"
  global_colors_info="{}"]
[/et_pb_wc_cart_notice]
```

```
[et_pb_wc_cart_notice
  page_type="cart"
  _builder_version="4.27.6"
  _module_preset="default"
  global_colors_info="{}"]
[/et_pb_wc_cart_notice]
```

Variante stylee complète (`page_type` par défaut = checkout/notices) :
```
[et_pb_wc_cart_notice
  required_field_indicator_color="#E02B20"
  placeholder_color="#000000"
  _builder_version="4.27.6"
  _module_preset="default"
  background_color="#FFFFFF"
  form_field_background_color="#E02B20"
  form_field_focus_background_color="#E02B20"
  form_field_focus_text_color="#FFFFFF"
  form_field_custom_margin="5px||7px||false|false"
  form_background_color="#E02B20"
  form_custom_margin="4px|8px|4px|6px|false|false"
  form_custom_padding="7px|5px|4px|5px|false|false"
  title_font="Montserrat|200|on||||||"
  title_text_color="#E02B20"
  title_font_size="16px"
  title_letter_spacing="1px"
  title_line_height="1.6em"
  error_font="Montserrat||||||||"
  error_text_color="#000000"
  error_font_size="17px"
  error_letter_spacing="-1px"
  error_line_height="1.6em"
  content_font="Cormorant Garamond||||||||"
  content_text_color="#000000"
  link_text_color="#E02B20"
  link_font_size="17px"
  link_letter_spacing="1px"
  link_line_height="1.6em"
  field_label_font="Cormorant Garamond||||||||"
  field_label_text_align="left"
  field_label_text_color="#000000"
  field_label_font_size="16px"
  field_label_letter_spacing="-1px"
  field_label_line_height="2.1em"
  custom_button="on"
  button_text_size="18px"
  button_text_color="#E02B20"
  button_bg_use_color_gradient="on"
  button_bg_color_gradient_type="elliptical"
  button_use_icon="off"
  border_radii="on|2px|2px|2px|2px"
  global_colors_info="{}"]
[/et_pb_wc_cart_notice]
```

`page_type` valeurs : `product` `cart` `checkout` (défaut si omis)
