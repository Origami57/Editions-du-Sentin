---
name: web-dev-diagnostic
description: "Diagnostic-first methodology for web development (CSS/JS/WordPress/WooCommerce). Use when troubleshooting or implementing frontend customizations, requiring systematic problem identification before coding solutions. Enforces strict workflow - diagnose → code → verify with maximum 3 attempts per approach."
---

# Web Development - Diagnostic-First Approach

## Workflow obligatoire

```
Phase -1 → Phase 0 → Phase 1 → Phase 2 → Phase 3
```

### Phase -1 : Lire le code fourni

Avant tout : lire et analyser TOUT le code déjà présent dans la conversation. Pour CSS : cartographier la cascade complète sur tous les breakpoints, identifier les conflits `!important`, les styles inline, les héritages. **Cette phase résout 80% des bugs sans diagnostic.**

### Phase 0 : Questions ciblées

Si des infos manquent : symptômes exacts, device/viewport, ce qui a changé récemment. Ne pas demander ce qu'on peut diagnostiquer soi-même.

### Phase 1 : Diagnostic

**Avec Chrome MCP disponible (méthode prioritaire) :**
1. Injecter le diagnostic via `javascript_tool` — ne jamais demander à l'utilisateur d'ouvrir la console
2. Si une interaction utilisateur est nécessaire (clic, scroll, ajout au panier...) : injecter les listeners d'abord, dire à l'utilisateur quelle action précise effectuer, lire avec `read_console_messages`
3. Toujours `clear: true` au premier `read_console_messages` ; toujours fournir un `pattern`
4. **Être cohérent** : soit Claude agit lui-même, soit il demande à l'utilisateur — jamais les deux

**Sans Chrome MCP :**
- Fournir le code diagnostic à coller en console
- Sur mobile sans console : `alert()` + `setTimeout`, jamais d'événements click

**Règles diagnostic :**
- Résultats dans un seul objet JSON
- Sélecteurs spécifiques, jamais génériques
- MutationObserver en premier quand un framework (Divi, WooCommerce...) est impliqué — ne jamais supposer son comportement

### Phase 2 : Solution

- **Une seule approche** par tentative — jamais de variantes sans données justificatives
- CSS OU JS sur une propriété — jamais les deux simultanément
- Modifications ciblées uniquement : `str_replace` ou diff exact. **Jamais de fichier complet pour changer 3 caractères**
- Auto-vérification : ce code contredit-il une règle existante ?
- Pour les conflits framework : soit laisser le framework gérer (CSS only), soit prendre le contrôle total en JS — jamais mixer

### Phase 3 : Vérification

- Échec après 3 tentatives → changer de stratégie, le dire explicitement
- Nouvelle tentative = nouveau diagnostic avec des données différentes

---

## Règles JS critiques

- JS inline styles + CSS `:hover` sur même propriété → conflit garanti — choisir l'un ou l'autre
- Pour rendre le contrôle à CSS depuis JS : `el.style.prop = ''` (jamais une valeur fixe)
- MutationObserver sur `document.body` avec `subtree:true` peut interférer avec le scroll WooCommerce/Bodycommerce — toujours annuler le scroll dans le callback si nécessaire (`$('html,body').stop(true,false)`)
- Une `SyntaxError` dans un `<script>` inline fait tomber TOUT le bloc — en cas de bug inexplicable, lire les erreurs console en premier avant tout autre diagnostic
- `jQuery(function($){ ... })` : toujours vérifier que le `});` de fermeture est présent

## WordPress/WooCommerce

- Toujours tester en navigation privée (élimine admin bar et cache)
- Admin bar : +32px desktop, +46px mobile sur les positions `fixed`
- LWS Optimize et autres plugins de cache : vider après chaque modif CSS/JS
- Bodycommerce : le CSS global Divi (Thème Options → Intégration) est la seule surface fiable pour les overrides
- Fragments WooCommerce : le fragment injecté reflète l'état du panier au moment de la requête — si un produit était déjà présent, son ajout incrémente la quantité sans créer de nouvel item

## Espacements CSS — règle obligatoire

**Avant d'injecter tout CSS avec des valeurs de `margin`, `padding`, `gap` ou `top/bottom/left/right` :**

1. Mesurer d'abord les espacements existants avec `getComputedStyle` et `getBoundingClientRect` sur les éléments adjacents
2. Déduire la valeur nécessaire à partir des mesures — jamais proposer une valeur arbitraire (ex: `margin-top: 10px`) sans avoir mesuré
3. Si le contexte est un composant tiers (WooCommerce, Divi...) : toujours mesurer le padding/margin interne déjà appliqué avant d'ajouter les propres valeurs

```js
// Pattern obligatoire avant tout espacement CSS
var el = document.querySelector('.mon-element');
var adjacent = document.querySelector('.element-adjacent');
var rEl = el.getBoundingClientRect();
var rAdj = adjacent.getBoundingClientRect();
var gap = rAdj.top - rEl.bottom; // espacement actuel entre les deux
var cs = getComputedStyle(el);
// → utiliser gap et cs.paddingBottom/marginBottom pour calculer la valeur correcte
```

## Audit visuel — règles MCP

**Avant tout screenshot MCP**, toujours exécuter via `javascript_tool` :
```js
document.querySelectorAll('.wpa-test-msg, #wpadminbar').forEach(el => el.style.display='none');
document.documentElement.style.marginTop='0';
```

**Limite viewport MCP :** le viewport est plafonné à ~528-1427px selon la fenêtre — jamais pleine résolution desktop. Conséquences :
- Ne jamais auditer header/layout global depuis les screenshots MCP seuls
- Pour audit visuel à pleine résolution : utiliser `web_fetch` pour le HTML/contenu + captures PNG transmises par l'utilisateur
- Screenshots MCP utiles uniquement pour : vérifier rendu CSS après modification, confirmer position/alignement d'un élément ciblé

**Méthode correcte pour un audit de page :**
1. `web_fetch` sur l'URL (fournie par l'utilisateur) → structure et contenu complets
2. Captures PNG de l'utilisateur → rendu visuel à pleine résolution
3. `javascript_tool` → valeurs CSS computées si nécessaire
4. Screenshots MCP → vérification ciblée post-modification uniquement

## Économie de contexte

- **Screenshots** : `zoom` ciblé sans `save_to_disk` uniquement — jamais plein écran
- **Diagnostic** : `getComputedStyle` + JS d'abord — screenshot seulement pour validation finale
- **Retours JS** : filtrer les résultats, ne jamais retourner de blocs >300 chars bruts
- **Recharger** la page si l'utilisateur a fait des modifications entre deux échanges
- **Une tâche = une conversation** — ne pas accumuler les sujets

## Format des modifications

- Localisation : toujours "entre `<extrait unique A>` et `<extrait unique B>`" — jamais de numéros de ligne seuls
- Petit changement (< 5 lignes) : donner uniquement le diff, pas le fichier complet
- Grand changement structurel (> 50% du fichier) : fichier complet justifié
