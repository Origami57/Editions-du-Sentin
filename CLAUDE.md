# CLAUDE.md — Éditions du Sentin

Ce fichier fournit le contexte nécessaire à Claude Code pour travailler efficacement sur ce dépôt.

## Vue d'ensemble

Dépôt de travail pour le site **editionsdusentin.com**, maison d'édition française (livres de Michel Clerc, Christophe Maruffy, etc.). Le dépôt ne contient **pas le code source WordPress** — il sert de référentiel pour :

- les fichiers exportés depuis WordPress (layouts Divi JSON, CSS personnalisé, snippets `functions.php`, JS injecté) ;
- les assets (logo, bannière, couvertures des livres) ;
- les exports CSV WooCommerce ;
- la documentation / historique des personnalisations faites sur le site en production.

Toute modification effective du site doit se faire via l'admin WordPress (ou via le MCP WordPress, voir plus bas), pas en éditant les fichiers du dépôt — ceux-ci sont des copies de référence.

## Stack technique

| Composant | Version / détail |
|---|---|
| CMS | WordPress |
| Thème | **Divi 4.27.6** (thème enfant — voir `functions.php.txt`, `wp_enqueue_style('wpm-Divi-style', …)`) |
| E-commerce | **WooCommerce** |
| Extension Divi/WC | **BodyCommerce** (Divi BodyShop for WooCommerce) — ajoute notamment des onglets produit personnalisés |
| Paiement | **WooPayments / Stripe** (gateway `woocommerce_payments`) |
| SEO | Yoast SEO (métas `_yoast_wpseo_*` visibles dans l'export) |
| Partage social | Monarch (Elegant Themes) |
| Cookies | Complianz (`cmplz-*`) |
| Livraison | La Poste Pro Expédition (points relais — meta `laposteproexp_parcel_point`) |
| Hébergement | **LWS** |
| Régime TVA | **Franchise en base — article 293 B du CGI** (TVA non applicable) |

### URLs

- Site de production : <https://www.editionsdusentin.com>
- Page d'accueil en cours de refonte : <https://editionsdusentin.com/accueil-test2>

## Accès MCP

> ⚠️ **Important pour Claude** : au démarrage de chaque session, vérifier via `ToolSearch` quels outils MCP sont réellement chargés. L'utilisateur mentionne parfois un « MCP WordPress connecté », mais dans les sessions observées jusqu'ici **seul le MCP GitHub** (`mcp__github__*`) est exposé. Si les outils WordPress n'apparaissent pas dans `ToolSearch`, ne pas inventer d'appels — **demander à l'utilisateur** de confirmer la connexion ou utiliser `WebFetch` sur les URLs publiques en lecture seule.

Quand le MCP WordPress **est** disponible, l'utiliser en priorité pour :
- lister et lire les pages, articles et produits ;
- récupérer les réglages (options, menus, widgets) ;
- appliquer des modifications validées par l'utilisateur.

## Arborescence du dépôt

```
/
├── CLAUDE.md                ← ce fichier
└── ES website/
    ├── Accueil-test2 26 mars.json   ← layout Divi JSON de la page d'accueil en refonte (~793 Ko)
    ├── Footer.json                  ← layout Divi JSON du footer (vide à ce jour)
    ├── Custom css.txt               ← CSS personnalisé global (2405 lignes)
    ├── functions.php.txt            ← snippets PHP du thème enfant (674 lignes)
    ├── js vendredi.txt              ← JS injecté via hooks <head> et <body>
    ├── wc-product-export-15-3-2026-*.csv  ← export WooCommerce (5 produits)
    ├── Logo_Editions_du_Sentin.png
    ├── Bannière-sentin.png
    └── *.jpg                        ← couvertures des livres
```

## Catalogue produits (extrait de l'export WooCommerce)

Cinq livres au catalogue, tous de type `simple`, catégorie `Sans catégorie`, marque associée à l'auteur :

| ID | Titre | Auteur | Prix | Prix promo |
|---|---|---|---|---|
| 1605 | La peur sans frontières | Michel Clerc | — | 14,99 € |
| 2441 | Une aventure humaine | Christophe Maruffy | 18 € | — |
| 2452 | Bakchich | Michel Clerc | — | 19,25 € |
| 2455 | Tapis rouge | Michel Clerc | — | 14,99 € |
| 2456 | Les hommes mariés | Michel Clerc | — | 14,99 € |

Chaque fiche produit utilise :
- un onglet personnalisé BodyCommerce **"Caractéristiques"** (pages, ISBN, poids, dimensions) via la meta `divi-bodyshop-woo_product_custom_tab_1*` ;
- Yoast pour la meta description ;
- ventes croisées et produits suggérés pointant vers les autres titres du catalogue.

## Personnalisations PHP clés (`ES website/functions.php.txt`)

### Shortcodes métier
- `[nom_produit id=""]`, `[description_produit]`, `[prix_produit]`, `[etiquettes_produit separateur=""]`
- `[categorie_produit]`, `[categorie_produit_courant]`, `[sous_categorie_produit]`, `[sous_categorie_produit_courant]`
- `[genre_produit]` → concatène catégorie + sous-catégorie ("Roman · Thriller")
- `[tag_desc]` → affiche la description du tag produit sur les archives `product_tag`

### WooCommerce — panier / commande
- Miniatures panier en `full` (pas de crop) via `woocommerce_cart_item_thumbnail`
- Suppression de l'onglet "Informations complémentaires" sur les fiches produit
- Traduction forcée de "Cross-sell products" → **"Vous aimerez peut-être aussi :"** (filtre `gettext`)
- Page panier : affichage du total **sans** frais de port (filtre `woocommerce_calculated_total`)
- Checkout : frais de port masqués tant que l'adresse n'est pas complète (filtre `woocommerce_cart_ready_to_calc_shipping` + JS sur `.wc-block-checkout__shipping-option`)

### WooPayments / Stripe
- **Frais de transaction** automatiquement ajoutés au panier :
  `fdt = ((0.015 × total + 0.25) − (0.0015 × total + 0.02)) × 1.20` (HT → TTC 20 %)
  Hooké sur `woocommerce_store_api_cart_calculate_fees` (Blocks) **et** `woocommerce_cart_calculate_fees` (legacy).
- Personnalisation de l'apparence UPE Stripe (`wcpay_upe_appearance`) — palette :
  `colorPrimary #e27865`, `colorBackground #f3efe6`, `colorText #2b2d2f`, font `Assistant`, `borderRadius 4px`.

### Page "Commande reçue" (order-received)
- Enqueue conditionnel de `css/confirmation.css` et `js/confirmation.js`
- Message de remerciement personnalisé (`woocommerce_thankyou_order_received_text`)
- Réécriture du libellé "Expédition" + ajout d'une ligne "Moyen de paiement" (avec marque + 4 derniers chiffres de la carte récupérés via `_wcpay_payment_method_details`)
- Masquage intelligent de l'adresse de livraison si identique à la facturation ou si point relais

### Emails WooCommerce
- Labels réécrits : `Total¹`, `Expédition : <méthode>`, ajout ligne moyen de paiement
- Note bas de tableau : `1 - T.V.A. non-applicable, article 293B du C.G.I.`
- Suppression des liens `tel:`, `mailto:` et Cloudflare email-protection dans le contenu
- Bloc HTML "Point de retrait" généré depuis `laposteproexp_parcel_point` (nom, adresse, horaires FR)

### Archives de tags produit
- Filtre `woocommerce_product_query_tax_query` forçant le filtrage par slug de tag
- Injection manuelle de la sidebar Monarch en footer (`et_social_sidebar_networks`) — palliatif à un bug d'affichage sur `is_tax('product_tag')`

## CSS / design tokens (`ES website/Custom css.txt`)

Couleurs principales utilisées à travers le site :

| Usage | Valeur |
|---|---|
| Fond global | `#fff9ef` (override `!important` sur `body`, `#page-container`, etc.) |
| Fond secondaire / inputs | `#f3efe6` |
| Texte | `#2b2d2f` |
| Orange principal (liens, hover) | `#d18000` → hover `#966205` |
| Accent corail (Stripe, erreurs) | `#e27865` |
| Marron (minicart, ondes) | `#ad9267` |
| Crème (texte bouton contact) | `#fff3dc` |

Polices : **Oswald** (Google Fonts, 400/500/600/700), **Assistant**, **Source Sans Pro**. Les icônes utilisent `ETmodules` (fournie par Divi).

Le CSS couvre notamment : bannière cookies Complianz, minicart avec animation d'ondes (`body.cart-wave`), icônes sociales du menu, bouton contact personnalisé dans le header.

## JavaScript injecté (`ES website/js vendredi.txt`)

Trois scripts inlinés (head/body via Divi Code Integration) :
1. Smooth scroll vers `#echo-ticker` depuis tout lien `href="#echo-ticker"` (offset 200px, easing quadratique, 800 ms).
2. Injection de l'année courante dans `#current-year`.
3. Calcul d'ancienneté via attribut `data-depuis` — si l'élément est un `<span>` nu, texte brut ; sinon HTML avec `<sup>ans</sup>`.

## Conventions & bonnes pratiques

- **Langue** : tout le contenu visible (interface, emails, messages) est en **français**. Respecter le ton et la typographie (espaces insécables, `«   »`, `&#8239;`).
- **Modifications PHP** : le fichier `functions.php.txt` est un **snapshot** — le "vrai" fichier vit dans le thème enfant Divi sur le serveur LWS. Toute modification doit être répercutée côté serveur (via SFTP LWS, admin WP ou MCP WordPress) **et** le snapshot du dépôt mis à jour en parallèle.
- **Divi builder** : privilégier les modules Divi natifs et les shortcodes métier listés ci-dessus plutôt que du HTML en dur, pour que les contenus restent éditables depuis le builder.
- **BodyCommerce** : pour les onglets produit, utiliser les metas `divi-bodyshop-woo_product_custom_tab_1*` (déjà en place pour "Caractéristiques").
- **Frais de transaction** : ne pas modifier la formule `calcul_fdt()` sans vérifier la grille tarifaire Stripe/WooPayments en vigueur.
- **TVA** : le site est en franchise en base — **ne jamais activer** le calcul de TVA ni ajouter de ligne "dont TVA" dans les emails / factures.
- **Git** : développer sur la branche `claude/create-claude-md-3TAKV` (ou celle explicitement demandée par l'utilisateur). Ne jamais pousser directement sur `main`/`master` sans instruction.

## Workflow de session recommandé pour Claude

1. **Démarrage** : lister les outils MCP via `ToolSearch` (en particulier chercher `wordpress`, `wp`, `mcp`). Signaler à l'utilisateur si le MCP WordPress est absent.
2. **Contexte** : lire ce `CLAUDE.md` + le fichier pertinent dans `ES website/` pour la tâche en cours.
3. **Lecture live du site** : utiliser `WebFetch` sur une URL publique (`/accueil-test2`, fiche produit, etc.) pour vérifier l'état en production — attention, le front n'expose pas tout (réglages, metas privées). Pour ces éléments, exiger le MCP WordPress ou demander à l'utilisateur.
4. **Modifications** : proposer les changements sous forme de diff / extrait + indiquer **où** l'appliquer côté serveur (thème enfant, Divi builder, réglage WP, option WC…).
5. **Commit** : commits descriptifs en français, sans co-author footer non sollicité.
