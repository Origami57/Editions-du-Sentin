# live-editing.md — Édition live Chrome MCP + REST API

## Workflow

```
1. Lire CSS/JS/PHP/modules via endpoints sentin (voir SKILL.md)
2. Injecter CSS de test en live (javascript_tool)
3. Valider visuellement (zoom ciblé, barre admin masquée)
4. Sauvegarder via CodeMirror + bouton save Divi (seule méthode fiable)
```

> ⚠️ **Aucune méthode REST directe ne fonctionne de façon fiable pour écrire le JS global Divi.**
> L'option `divi_integration_body` est stockée dans `wp_options` sous la clé `et_divi` comme tableau PHP sérialisé. Un POST REST ne peut pas écrire dans ce tableau sans risquer d'écraser les autres champs. La seule méthode validée est CodeMirror + bouton save depuis l'onglet **L'intégration**.

---

## Lire le contenu d'une page ou layout

### Post content brut
```javascript
var r = new XMLHttpRequest();
r.open('GET', '/wp-json/sentin/v1/page?id=9334', false);
r.withCredentials = true;
r.setRequestHeader('X-WP-Nonce', window.wpApiSettings.nonce);
r.send();
var content = JSON.parse(r.responseText).post_content;
```

### Post content décodé (line_break_holder + entités HTML résolues)
```javascript
var r = new XMLHttpRequest();
r.open('GET', '/wp-json/sentin/v1/page-full?id=9334', false);
r.withCredentials = true;
r.setRequestHeader('X-WP-Nonce', window.wpApiSettings.nonce);
r.send();
var data = JSON.parse(r.responseText);
// data.post_content = brut
// data.content_decoded = décodé et lisible
```

### Modules et_pb_code — MÉTHODE OBLIGATOIRE
Ne jamais lire via DOM (innerHTML bloqué par filtre MCP). Toujours via l'endpoint.

```javascript
var r = new XMLHttpRequest();
r.open('GET', '/wp-json/sentin/v1/code-modules?id=10620', false);
r.withCredentials = true;
r.setRequestHeader('X-WP-Nonce', window.wpApiSettings.nonce);
r.send();
var data = JSON.parse(r.responseText);
// data.count = nombre de modules
// data.modules[i].index = position dans le layout
// data.modules[i].attrs = attributs Divi (module_class, etc.)
// data.modules[i].content = HTML/JS/CSS décodé et lisible
```

### Tous les layouts Theme Builder
```javascript
var r = new XMLHttpRequest();
r.open('GET', '/wp-json/sentin/v1/all-layouts', false);
r.withCredentials = true;
r.setRequestHeader('X-WP-Nonce', window.wpApiSettings.nonce);
r.send();
var layouts = JSON.parse(r.responseText).layouts;
// layouts[i] = { id, title, type, status, code_modules[], content_decoded }
```

---

## Champs CSS Divi

| Champ UI | Attribut shortcode | Usage |
|---|---|---|
| Avant | `custom_css_before` | `::before` — inclure `content:''` |
| Élément principal | `custom_css_main_element` | CSS sans sélecteur |
| Après | `custom_css_after` | `::after` — inclure `content:''` |
| Forme libre | `custom_css_free_form` | Règles avec sélecteurs complets |

---

## Injecter / modifier / supprimer un CSS de test

```javascript
// Injecter
const s = document.createElement('style');
s.id = 'test-css';
s.textContent = `.ma-classe { prop: val !important; }`;
document.head.appendChild(s);

// Modifier
document.getElementById('test-css').textContent = '...';

// Supprimer
document.getElementById('test-css').remove();
```

---

## Vérification visuelle

```javascript
document.getElementById('wpadminbar').style.display = 'none';
document.documentElement.style.marginTop = '0';
document.querySelector('.ma-section').scrollIntoView({behavior:'instant'});
```

Puis `zoom(region)` sans `save_to_disk`.

---

## Sauvegarder le post_content d'une page

```javascript
var r = new XMLHttpRequest();
r.open('POST', '/wp-json/sentin/v1/page-update', false);
r.withCredentials = true;
r.setRequestHeader('Content-Type', 'application/json');
r.setRequestHeader('X-WP-Nonce', window.wpApiSettings.nonce);
r.send(JSON.stringify({ id: 9334, content: newContent }));
```

---

## Divi Options du thème — CSS et JS globaux

URL : `/wp-admin/admin.php?page=et_divi_options`

| Contenu | Nom du champ HTML |
|---|---|
| CSS global | `divi_custom_css` |
| JS global (avant `</body>`) | `divi_integration_body` |
| JS global (dans `<head>`) | `divi_integration_head` (vide sur ce site) |

### ⚠️ CodeMirror — obligatoire

Ces textareas sont gérés par CodeMirror. Modifier `.value` directement **ne persiste pas** à la sauvegarde. Toujours passer par l'instance CodeMirror.

```javascript
// Trouver l'instance par un marqueur de contenu connu
var cm = null;
var allCM = document.querySelectorAll('.CodeMirror');
for (var i = 0; i < allCM.length; i++) {
  if (allCM[i].CodeMirror && allCM[i].CodeMirror.getValue().indexOf('MON_MARQUEUR') !== -1) {
    cm = allCM[i].CodeMirror;
    break;
  }
}
// Lire
var current = cm.getValue();
// Modifier
cm.setValue(current.replace('ancien', 'nouveau'));
```

### ⚠️ Onglet obligatoire selon le type de modification

| Type de modification | Onglet requis |
|---|---|
| JS global (`divi_integration_body`) | **L'intégration** |
| CSS global (`divi_custom_css`) | **Général** |

**RÈGLE ABSOLUE** : Toute sauvegarde depuis le mauvais onglet écrase le contenu avec une version vide ou tronquée.

Workflow obligatoire :
1. Naviguer vers `/wp-admin/admin.php?page=et_divi_options`
2. Cliquer physiquement sur le bon onglet et prendre un screenshot pour confirmer
3. Attendre 2s que CodeMirror charge
4. Faire la modification via `cm.setValue()` + `cm.save()` + synchronisation textarea
5. Vérifier visuellement avec un screenshot que le bon onglet est toujours actif
6. Demander à l'utilisateur de sauvegarder **sans changer d'onglet**

`cm.setValue()` peut provoquer un retour automatique sur l'onglet Général — toujours vérifier l'onglet actif après la modification et recliquer sur le bon onglet si nécessaire.

### ⛔ Claude ne sauvegarde jamais seul le CSS/JS global

### ✅ Workflow de sauvegarde — rôles

C'est Claude qui :
1. Navigue vers `/wp-admin/admin.php?page=et_divi_options`
2. Clique sur le bon onglet (Général pour CSS, L'intégration pour JS)
3. Attend 2s que CodeMirror charge
4. Fait la modification via `cm.setValue()` + `cm.save()` + synchronisation textarea
5. Prend un screenshot pour confirmer que le bon onglet est toujours actif
6. Dit à l'utilisateur : **"Tu peux sauvegarder."**

C'est l'utilisateur qui clique sur "Sauvegarder les changements" — jamais Claude.

**Toutes les tentatives autonomes ont échoué** : REST API, FormData POST, clic programmatique sur le bouton save. Résultats typiques : JS tronqué, champs vidés, modifications perdues.

**Règle absolue** : après avoir modifié le CodeMirror, Claude s'arrête et demande à l'utilisateur de cliquer sur "Sauvegarder les changements". Claude ne clique jamais sur le bouton save lui-même.

Après sauvegarde par l'utilisateur, régénérer le snapshot CSS :
```javascript
var r = new XMLHttpRequest();
r.open('POST', '/wp-json/sentin/v1/css-file', false);
r.withCredentials = true;
r.setRequestHeader('X-WP-Nonce', window.wpApiSettings.nonce);
r.send('{}');
```

---

## Obtenir un nonce REST valide

Depuis une page admin :
```javascript
// Option 1 — depuis wpApiSettings (disponible sur la plupart des pages admin)
var nonce = window.wpApiSettings.nonce;

// Option 2 — depuis admin-ajax (toujours disponible)
var r = new XMLHttpRequest();
r.open('GET', '/wp-admin/admin-ajax.php?action=rest-nonce', false);
r.withCredentials = true;
r.send();
var nonce = r.responseText.trim();
```

---

## Pièges

| Problème | Solution |
|---|---|
| `::after` sans effet | Ajouter `content:''` |
| `%%` non interprété en live | Utiliser `%` simples |
| Override sans effet | Augmenter la spécificité ou `!important` |
| `background_blend=multiply` + fond sombre | Utiliser `soft-light` |
| Rendu admin ≠ visiteur | Masquer la barre admin ou tester en privé |
| `filter:drop-shadow` sur parent | S'applique au composite — déplacer sur l'élément cible |
| innerHTML modules code bloqué | Utiliser `/wp-json/sentin/v1/code-modules?id=XXX` |
| CodeMirror `.value` non persisté | Passer par `cm.setValue()` puis bouton save |
| Sauvegarde JS perdue | `cm.setValue()` remet l'onglet sur Général — toujours vérifier + recliquer L'intégration avant save |
| Filtre MCP bloque le contenu | Lire via endpoint sentin, pas via DOM |
