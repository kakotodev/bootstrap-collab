# Documentation technique — VoyageAventure

Cette documentation décrit **le site tel qu'il est construit aujourd'hui** : son architecture, le contenu de chaque page, le fonctionnement du CSS et du JavaScript, et les tâches les plus courantes (ajouter une destination, modifier le menu…).

Elle complète les deux autres documents du dépôt :

| Document | Rôle |
|---|---|
| [README.md](../README.md) | Présentation rapide du projet et lancement |
| [GUIDE-STYLE.md](../GUIDE-STYLE.md) | Règles à respecter : couleurs, typographie, composants autorisés, plan de chaque page |
| **DOCUMENTATION.md** (ce fichier) | Fonctionnement du code existant et procédures |

---

## Sommaire

1. [Présentation](#1-présentation)
2. [Installation et lancement](#2-installation-et-lancement)
3. [Arborescence](#3-arborescence)
4. [Architecture d'une page](#4-architecture-dune-page)
5. [Les pages en détail](#5-les-pages-en-détail)
6. [La feuille de style `css/style.css`](#6-la-feuille-de-style-cssstylecss)
7. [JavaScript](#7-javascript)
8. [Accessibilité](#8-accessibilité)
9. [Référencement (SEO)](#9-référencement-seo)
10. [Procédures courantes](#10-procédures-courantes)
11. [Travailler à plusieurs avec Git](#11-travailler-à-plusieurs-avec-git)
12. [État du projet et points à corriger](#12-état-du-projet-et-points-à-corriger)

---

## 1. Présentation

**VoyageAventure** est le site vitrine d'une agence de voyages fictive, basée à Paris et fondée en 2015. Le site comporte 4 pages : Accueil, Destinations, À propos et Contact.

Le projet est une **refonte** : un site HTML/CSS existant a été reconstruit avec **Bootstrap 5.3**. Les objectifs sont les suivants :

- un design **responsive**, pensé d'abord pour le mobile ;
- un code **accessible** (navigation au clavier, lecteurs d'écran, contrastes) ;
- **le moins de CSS personnalisé possible** : Bootstrap fournit la mise en page et les composants ;
- **presque aucun JavaScript** : seuls le script de Bootstrap et un petit script de validation du formulaire sont utilisés.

### Technologies

| Outil | Version | Chargement |
|---|---|---|
| HTML5 / CSS3 | — | fichiers locaux |
| Bootstrap (CSS + JS bundle) | 5.3.8 | CDN jsDelivr, avec empreinte `integrity` |
| Bootstrap Icons | 1.13.1 | CDN jsDelivr, avec empreinte `integrity` |
| Formspree | — | service externe d'envoi du formulaire (pas encore configuré) |

Le projet n'utilise **ni outil de build, ni npm, ni framework JavaScript**.

---

## 2. Installation et lancement

```bash
git clone https://github.com/kakotodev/bootstrap-collab.git
cd bootstrap-collab
```

Ensuite, au choix :

- ouvrir `index.html` directement dans un navigateur ;
- **ou (recommandé)** utiliser l'extension VS Code **Live Server** (clic droit sur `index.html` → *Open with Live Server*). La page se recharge alors à chaque enregistrement.

Une **connexion internet est nécessaire**, car Bootstrap et ses icônes sont chargés depuis un CDN. Sans connexion, les pages s'affichent sans mise en forme.

### Navigateurs pris en charge

Tous les navigateurs récents (Chrome, Edge, Firefox, Safari). Le filtre des destinations utilise le sélecteur CSS `:has()`, disponible dans tous ces navigateurs depuis fin 2023. Sur un navigateur plus ancien, le filtre ne fait rien : toutes les destinations restent affichées, mais la page reste utilisable.

---

## 3. Arborescence

```
bootstrap-collab/
├── index.html            Accueil : hero, services, témoignages
├── destinations.html     Destinations : filtres par continent, 6 cartes, 6 modales de détail
├── apropos.html          À propos : histoire, chiffres clés, valeurs, équipe
├── contact.html          Contact : coordonnées et formulaire de demande
├── css/
│   └── style.css         Thème Bootstrap + quelques règles propres au site
├── img/
│   ├── favicon.svg       Icône du site (globe bleu, vectoriel)
│   └── hero.webp         Photo de fond de l'accueil (1920 × 1080)
├── docs/
│   └── DOCUMENTATION.md  Ce fichier
├── GUIDE-STYLE.md        Charte graphique et règles de construction
└── README.md             Présentation du projet
```

Dossiers d'images prévus par le guide de style, **pas encore créés** : `img/destinations/`, `img/apropos/`, `img/equipe/`.

---

## 4. Architecture d'une page

Les 4 pages ont **exactement le même squelette**. Seul le contenu de `<main>` change d'une page à l'autre.

```
<html lang="fr">
├── <head>
│   ├── meta charset, viewport
│   ├── <title> et meta description        ← propres à chaque page
│   ├── balises Open Graph (og:*)          ← og:title et og:description propres à chaque page
│   ├── favicon
│   ├── Bootstrap CSS + Bootstrap Icons (CDN)
│   └── css/style.css                      ← toujours APRÈS Bootstrap
└── <body>
    ├── Lien d'évitement « Aller au contenu »
    ├── <header class="sticky-top">        ← commun
    │   └── <nav class="navbar navbar-expand-lg bg-dark">
    ├── <main id="contenu">                ← propre à chaque page
    ├── <footer class="bg-dark">           ← commun
    ├── Bootstrap JS bundle (CDN)
    └── (contact.html uniquement) script de validation
```

### 4.1 En-tête et navigation

- La barre reste en haut de l'écran au défilement grâce à `sticky-top`.
- En dessous de **992 px** (point de rupture `lg`), le menu se replie derrière un bouton burger (`navbar-toggler`). Ce comportement nécessite le JavaScript de Bootstrap.
- Le lien de la page courante porte `class="nav-link active" aria-current="page"`. **C'est la seule différence de l'en-tête d'une page à l'autre.**
- `data-bs-theme="dark"` passe les textes, liens et contours de focus en clair sur le fond bleu nuit.

### 4.2 Pied de page

Il est organisé en trois colonnes (une seule sur mobile) : présentation de l'agence, coordonnées dans une balise `<address>`, et plan du site. Une ligne de copyright termine le pied de page.

> ⚠️ **L'en-tête, le pied de page et le `<head>` sont copiés à l'identique dans les 4 fichiers.** Il n'y a pas d'inclusion automatique. Toute modification doit donc être reportée à la main dans **les 4 pages** (voir [§10.3](#103-modifier-len-tête-le-menu-ou-le-pied-de-page)).

### 4.3 Rythme des sections

Dans `<main>`, chaque section suit le même modèle : `<section class="py-5 …">` > `<div class="container">`. Les fonds alternent entre `bg-white` et `bg-body-tertiary`. `bg-dark` est réservé aux bandeaux de titre et aux chiffres clés. Les règles complètes sont dans le [guide de style, §5](../GUIDE-STYLE.md#5-espacements-et-rythme-des-sections).

---

## 5. Les pages en détail

### 5.1 Accueil — `index.html`

| # | Section | Fond | Contenu |
|---|---|---|---|
| 1 | Hero | photo `img/hero.webp` assombrie (classe `.hero`) | Titre `h1`, accroche, 2 boutons : « Découvrir nos voyages » → destinations, « Nous contacter » → contact |
| 2 | Nos services | blanc | 3 cartes à icône : Voyages organisés (`bi-airplane`), Réservations (`bi-building`), Voyages sur mesure (`bi-backpack`) |
| 3 | Ils nous font confiance | gris clair | 2 témoignages : `<figure>` + `<blockquote>` + `<figcaption>` |

Sur mobile, les boutons du hero s'empilent (`flex-column flex-sm-row`) et les cartes passent sur une seule colonne.

### 5.2 Destinations — `destinations.html`

C'est la page la plus riche. Elle se compose de trois parties.

**1. Le bandeau de titre** « Nos destinations ».

**2. Les filtres et la liste**, dans `<section class="destinations">`. Le nom de classe `destinations` est **indispensable** au filtrage.

| Destination | Continent (`data-continent`) | Durée | Prix | Modale |
|---|---|---|---|---|
| Paris, France | `europe` | 7 jours | dès 599 € | `#detail-paris` |
| Tokyo, Japon | `asie` | 10 jours | dès 1 299 € | `#detail-tokyo` |
| New York, États-Unis | `amerique` | 5 jours | dès 899 € | `#detail-new-york` |
| Safari Kenya | `afrique` | 12 jours | dès 1 899 € | `#detail-kenya` |
| Rome, Italie | `europe` | 6 jours | dès 499 € | `#detail-rome` |
| Bali, Indonésie | `asie` | 14 jours | dès 999 € | `#detail-bali` |

La grille affiche 1 carte par ligne sur mobile, 2 à partir de 576 px et 3 à partir de 992 px.

**3. Les modales de détail**, placées à la fin de `<main>`. Chacune contient une description, la durée, le prix, un programme jour par jour (`list-group-numbered`) et un bouton « Demander un devis » qui mène à la page Contact.

#### Fonctionnement du filtre (sans JavaScript)

1. Chaque filtre est un bouton radio `<input type="radio" class="btn-check" name="continent" id="f-…">` suivi de son `<label class="btn btn-outline-primary rounded-pill">`. Bootstrap affiche le label comme un bouton et le met en surbrillance quand le radio est coché.
2. Chaque colonne de carte porte un attribut `data-continent="…"`.
3. Dans `style.css`, une règle par continent masque les autres colonnes :

```css
.destinations:has(#f-europe:checked) [data-continent]:not([data-continent="europe"]) {
    display: none;
}
```

Autrement dit : *si la section contient le radio « Europe » coché, alors toutes les colonnes dont le continent n'est pas « europe » sont masquées.* « Tous » (`#f-tous`) n'a pas de règle : quand il est coché, aucune colonne n'est masquée.

L'attribut `data-continent` est placé sur la **colonne** (`.col`) et non sur la carte, pour que la grille se referme sans laisser de trou.

### 5.3 À propos — `apropos.html`

| # | Section | Fond | Contenu |
|---|---|---|---|
| 1 | Bandeau « À propos de nous » | bleu nuit | — |
| 2 | Notre histoire | blanc | Texte (7/12) + emplacement photo (5/12). La photo est **provisoire** : une icône `bi-map` dans un cadre `ratio-4x3` |
| 3 | VoyageAventure en chiffres | bleu nuit | 4 chiffres dans une liste de définitions `<dl>` : 5 000+ voyages, 50+ destinations, 98 % de clients satisfaits, création en 2015 |
| 4 | Nos valeurs | gris clair | 3 cartes : Excellence, Confiance, Respect |
| 5 | Notre équipe | blanc | 3 membres : Jean Dupont, Marie Martin, Pierre Durand. Les portraits sont **provisoires** (`bi-person-circle`) |

Les chiffres clés utilisent `<dt>` pour le libellé et `<dd>` pour le chiffre. Un lecteur d'écran lit ainsi « Voyages organisés : 5 000+ ». La classe `flex-column-reverse` affiche visuellement le chiffre au-dessus du libellé.

Des commentaires HTML dans le code indiquent la balise `<img>` à mettre à la place de chaque élément provisoire.

### 5.4 Contact — `contact.html`

La page est organisée en deux colonnes sur grand écran (une seule sur mobile) :

- **Coordonnées** (`col-lg-5`) : adresse, téléphone (lien `tel:`), e-mail (lien `mailto:`) et horaires, dans une balise `<address>`.
- **Formulaire** (`col-lg-7`), dans une carte.

#### Champs du formulaire

| Champ | `name` | Type | Obligatoire | `autocomplete` |
|---|---|---|---|---|
| Nom complet | `nom` | text | oui | `name` |
| Adresse e-mail | `email` | email | oui | `email` |
| Téléphone | `telephone` | tel | non | `tel` |
| Destination souhaitée | `destination` | select : vide, `europe`, `asie`, `amerique`, `afrique`, `oceanie` | non | — |
| Budget par personne | `budget` | select : vide, `500-1000`, `1000-2000`, `2000-3000`, `3000+` | non | — |
| Message | `message` | textarea (hauteur 10rem) | oui | — |
| Recevoir les offres | `newsletter` | checkbox, valeur `oui`, **décochée par défaut** | non | — |

#### Envoi

Le formulaire est envoyé en `POST` vers **Formspree** (`https://formspree.io/f/VOTRE_ID`). **`VOTRE_ID` est un emplacement à remplacer** : tant qu'il n'est pas remplacé, l'envoi échoue (voir [§10.5](#105-activer-lenvoi-du-formulaire)).

#### RGPD

- La case newsletter est **facultative et décochée par défaut** : le consentement doit être un acte volontaire.
- Une mention sous le bouton explique l'usage des données et comment en demander la suppression.
- Le lien vers la politique de confidentialité sera ajouté quand la page `confidentialite.html` existera.

---

## 6. La feuille de style `css/style.css`

Le fichier est chargé **après** Bootstrap pour pouvoir le surcharger. Il contient trois types de règles.

### 6.1 Thème (bloc `:root`)

Les couleurs de Bootstrap sont remplacées par celles de la marque, grâce à ses variables CSS :

| Variable | Valeur | Rôle |
|---|---|---|
| `--bs-primary` / `--bs-primary-rgb` | `#1d6fa5` | Bleu principal : boutons, liens, icônes. Texte blanc dessus : contraste de 5,4:1 |
| `--bs-dark` / `--bs-dark-rgb` | `#2c3e50` | Bleu nuit : en-tête, pied de page, bandeaux |
| `--bs-link-color`, `--bs-link-hover-color` | `#1d6fa5`, `#175a86` | Liens |
| `--bs-focus-ring-color` | bleu à 40 % | Halo de focus des champs |
| `--bs-body-color` | `#333` | Texte courant |
| `--bs-body-line-height` | `1.6` | Interligne |

### 6.2 Surcharges de composants

| Règle | Pourquoi |
|---|---|
| `.btn-primary` | Avec le CSS de Bootstrap chargé par CDN, les couleurs des boutons sont fixées dans chaque variante et ne suivent pas `--bs-primary`. Elles sont donc redéfinies ici, avec des variantes plus foncées pour le survol et le clic. |
| `.btn-outline-primary` | Même raison, pour les boutons de filtre de la page Destinations. |
| `.form-floating > textarea.form-control` | Dans un `form-floating`, Bootstrap ignore l'attribut `rows` : la hauteur du message est fixée à 10rem. |

### 6.3 Règles propres au site

| Règle | Rôle |
|---|---|
| `.lien-evitement:focus` | Affiche le lien « Aller au contenu » en haut à gauche quand il reçoit le focus clavier |
| `:focus-visible` et `[data-bs-theme="dark"] :focus-visible` | Contour de focus épais (3 px) : bleu sur fond clair, blanc sur fond sombre |
| `.hero` | Photo de fond de l'accueil, recouverte d'un voile noir à 55 % pour que le texte blanc reste lisible |
| `.destinations:has(…)` | Filtres des destinations (voir [§5.2](#52-destinations--destinationshtml)) |

### 6.4 Code mort à supprimer

Plusieurs règles viennent de l'**ancien site** et ne sont plus utilisées par aucune page. Elles utilisent des couleurs en dur et des largeurs fixes, contraires au guide de style :

`.page-header`, `.page-title`, `.page-subtitle`, `.section-title`, `.services`, `.services-grid`, `.service-card`, `.service-icon`, `.service-title`, `.testimonials`, `.testimonials-grid`, `.testimonial-card`, `.testimonial-text`, `.testimonial-author`, ainsi que le contenu des blocs `@media (max-width: 768px)` (`.hero-title`, `.filter-buttons`) et `@media (prefers-reduced-motion: reduce)` (`.filter-btn`, `.destination-card`, `.destination-btn`).

Ces règles peuvent être supprimées sans aucun effet visible.

---

## 7. JavaScript

Le site utilise uniquement deux scripts :

1. **Bootstrap bundle** (CDN), sur toutes les pages. Il fait fonctionner le menu burger (`collapse`) et les modales de la page Destinations (`modal`).
2. **Validation du formulaire** (`contact.html`), un script repris de la documentation Bootstrap. À l'envoi, si un champ obligatoire est vide ou invalide, l'envoi est bloqué et la classe `was-validated` est ajoutée au formulaire. Bootstrap affiche alors les champs en erreur et leur message `invalid-feedback`.

Le formulaire porte l'attribut `novalidate` : les bulles d'erreur natives du navigateur sont désactivées au profit des messages Bootstrap, plus lisibles et traduits.

---

## 8. Accessibilité

Mesures déjà en place sur tout le site :

| Mesure | Mise en œuvre |
|---|---|
| Langue de la page | `<html lang="fr">` |
| Lien d'évitement | « Aller au contenu » → `#contenu`, visible uniquement au clavier |
| Structure sémantique | `header`, `nav` avec `aria-label`, `main`, `section`, `article`, `footer`, `address`, `figure`/`blockquote`, `dl` |
| Titres | Un seul `<h1>` par page, sans niveau sauté. La taille visuelle est donnée par une classe (`h5`, `display-5`…) |
| Icônes décoratives | `aria-hidden="true"` sur chaque `<i class="bi …">` |
| Boutons « Voir détails » | `aria-label` explicite (« Voir les détails de Paris, France ») |
| Modales | `aria-labelledby` relié au titre, bouton de fermeture avec `aria-label="Fermer"` |
| Filtres | `<fieldset>` + `<legend>` « Filtrer par continent », boutons radio natifs utilisables au clavier (flèches) |
| Formulaire | Un `<label>` par champ, `autocomplete`, messages d'erreur associés, champs facultatifs signalés |
| Focus | Contour visible de 3 px, adapté aux zones sombres |
| Contrastes | Bleu principal conforme WCAG AA (5,4:1). Textes secondaires en `text-body-secondary` |
| Typographie française | Espaces insécables (`&nbsp;`) avant `: ; ! ? €`, guillemets « » |

Avant de terminer une page, passer la [check-list du guide de style (§11)](../GUIDE-STYLE.md#11-check-list-avant-de-terminer-une-page).

---

## 9. Référencement (SEO)

Chaque page possède :

- un `<title>` de la forme `VoyageAventure - <Page>` ;
- une `meta description` unique, qui résume la page ;
- des balises **Open Graph** (`og:type`, `og:locale`, `og:site_name`, `og:title`, `og:description`) pour l'aperçu lors d'un partage sur les réseaux sociaux ;
- un favicon SVG.

Il manque encore une `og:image` (image d'aperçu), un `sitemap.xml` et un `robots.txt`. Ils seront utiles une fois le site en ligne.

---

## 10. Procédures courantes

### 10.1 Ajouter une destination

Dans `destinations.html` :

1. **Carte** : copier un bloc `<div class="col" data-continent="…">` complet dans la grille, puis modifier :
   - `data-continent` (valeurs possibles : `europe`, `asie`, `amerique`, `afrique`, `oceanie`) ;
   - la durée, le titre, la description et le prix (avec `&nbsp;` avant `€`) ;
   - `data-bs-target="#detail-<ville>"` et l'`aria-label` du bouton.
2. **Modale** : copier une modale complète en fin de `<main>`, puis modifier :
   - l'`id="detail-<ville>"` (identique au `data-bs-target` de la carte) ;
   - `aria-labelledby` et l'`id` du titre : `detail-<ville>-titre` ;
   - le texte, les badges et le programme (un `<li>` par jour).
3. **Nouveau continent ?** Si c'est la première destination d'un continent absent des filtres (par exemple l'Océanie), voir [§10.2](#102-ajouter-un-continent-au-filtre).
4. Mettre à jour la `meta description` et `og:description` si la destination est importante.

Les identifiants ne doivent contenir que des minuscules, sans accent ni espace, avec des tirets (`new-york`).

### 10.2 Ajouter un continent au filtre

1. Dans le `<fieldset>` de `destinations.html`, ajouter un couple radio + label :

   ```html
   <input type="radio" class="btn-check" name="continent" id="f-oceanie">
   <label class="btn btn-outline-primary rounded-pill" for="f-oceanie">Océanie</label>
   ```

2. Dans `css/style.css`, ajouter une ligne au sélecteur des filtres (attention à la virgule) :

   ```css
   .destinations:has(#f-oceanie:checked) [data-continent]:not([data-continent="oceanie"])
   ```

3. Vérifier que l'option existe aussi dans la liste « Destination souhaitée » du formulaire de contact.

### 10.3 Modifier l'en-tête, le menu ou le pied de page

Ces blocs sont dupliqués dans les 4 fichiers HTML. Pour ajouter une page au menu, par exemple :

1. Faire la modification dans une page.
2. Copier le bloc modifié dans les 3 autres pages.
3. Dans chaque page, remettre `active` et `aria-current="page"` sur le bon lien.
4. Vérifier le résultat : ouvrir les 4 pages et comparer, ou lancer `git diff` pour s'assurer que les 4 fichiers ont reçu le même changement.

### 10.4 Créer une nouvelle page

1. Dupliquer `apropos.html` ou `contact.html` (pages avec bandeau de titre).
2. Changer le `<title>`, la `meta description`, `og:title` et `og:description`.
3. Retirer `active` / `aria-current` du menu (ou les placer sur le nouveau lien).
4. Vider `<main>` en gardant le bandeau, puis construire les sections selon le [plan du guide de style (§9)](../GUIDE-STYLE.md#9-plan-de-chaque-page).
5. Ajouter le lien dans le menu et/ou le pied de page **des 4 pages** (voir [§10.3](#103-modifier-len-tête-le-menu-ou-le-pied-de-page)).

### 10.5 Activer l'envoi du formulaire

1. Créer un compte sur [formspree.io](https://formspree.io) et un nouveau formulaire, avec l'adresse de réception.
2. Copier l'identifiant fourni (par exemple `xyzabcd`).
3. Dans `contact.html`, remplacer `VOTRE_ID` dans l'attribut `action` du `<form>`.
4. Envoyer un message de test et le valider dans l'e-mail de confirmation de Formspree.

### 10.6 Ajouter une image

1. Convertir l'image en **WebP**, aux dimensions du [guide de style (§6)](../GUIDE-STYLE.md#6-images-et-icônes).
2. La nommer en minuscules, sans accent ni espace (`rome.webp`, `jean-dupont.webp`) et la placer dans le bon sous-dossier de `img/`.
3. Toujours renseigner `alt`, `width` et `height`, et ajouter `loading="lazy"` pour toute image autre que celle du hero.

### 10.7 Mettre à jour Bootstrap

Changer la version dans les 3 URL du CDN **et** remplacer les empreintes `integrity` par celles indiquées sur [getbootstrap.com](https://getbootstrap.com/docs/5.3/getting-started/download/#cdn-via-jsdelivr), dans les 4 pages. Si l'empreinte ne correspond pas au fichier, le navigateur refuse de le charger et le site perd sa mise en forme.

---

## 11. Travailler à plusieurs avec Git

Dépôt : **https://github.com/kakotodev/bootstrap-collab** (branche principale : `main`).

L'historique montre la méthode suivie jusqu'ici : **une branche par page ou par fonctionnalité**, fusionnée dans `main` par une *pull request* (PR #1 `feature/accueil`, #2 `destinations-refonte`, #3 `lam`).

Méthode recommandée :

```bash
git switch main
git pull                                   # partir de la dernière version
git switch -c feature/nom-de-la-tache      # une branche par tâche
# … modifications …
git add <fichiers>
git commit -m "Ajouter la destination Sydney"
git push -u origin feature/nom-de-la-tache
# puis ouvrir une pull request sur GitHub
```

Bonnes pratiques :

- **Faire un `git pull` avant de commencer**, pour réduire les conflits.
- **Faire des modifications de l'en-tête et du pied de page dans une PR à part** : elles touchent les 4 fichiers et entrent facilement en conflit avec le travail des autres.
- Écrire des messages de commit courts et à l'infinitif : « Ajouter… », « Corriger… ».
- Faire relire la PR par un autre membre de l'équipe avant de la fusionner, en s'appuyant sur la [check-list du guide de style](../GUIDE-STYLE.md#11-check-list-avant-de-terminer-une-page).

---

## 12. État du projet et points à corriger

### Terminé

- Socle commun : `<head>`, navigation responsive, pied de page, thème, favicon, SEO de base, accessibilité
- Accueil : hero avec photo, services, témoignages
- Destinations : 6 destinations, filtres par continent sans JavaScript, 6 modales avec programme
- À propos : histoire, chiffres clés, valeurs, équipe (avec images provisoires)
- Contact : coordonnées, formulaire avec validation

### Reste à faire

| Priorité | Tâche | Fichiers |
|---|---|---|
| Haute | Remplacer `VOTRE_ID` pour que le formulaire fonctionne ([§10.5](#105-activer-lenvoi-du-formulaire)) | `contact.html` |
| Haute | Créer les pages `mentions-legales.html` et `confidentialite.html` (obligatoires pour un site qui collecte des données), puis les relier depuis le pied de page et le formulaire | nouvelles pages + les 4 pages |
| Moyenne | Ajouter les vraies photos : destinations (600 × 400), histoire (800 × 600), équipe (240 × 240) | `img/`, `destinations.html`, `apropos.html` |
| Moyenne | Supprimer les règles CSS inutilisées ([§6.4](#64-code-mort-à-supprimer)) | `css/style.css` |
| Basse | Filtre Océanie : le formulaire de contact propose l'Océanie, mais aucune destination ni aucun filtre ne correspond. Ajouter une destination océanienne ([§10.1](#101-ajouter-une-destination), [§10.2](#102-ajouter-un-continent-au-filtre)) ou retirer l'option du formulaire | `destinations.html`, `css/style.css`, `contact.html` |
| Basse | Ajouter un message de confirmation après l'envoi (`alert alert-success`) | `contact.html` |
| Basse | Ajouter une `og:image`, un `sitemap.xml` et un `robots.txt` | toutes les pages |

### Écarts avec le guide de style

| Écart | Détail |
|---|---|
| Prix des destinations | Le guide (§8.4) prévoit `<span class="fw-bold">À partir de …</span>`. Le code utilise un badge `text-bg-light fs-6` « Dès … ». Il faut harmoniser l'un ou l'autre. |
| Images des cartes | Le guide prévoit une `card-img-top` par destination. Les cartes n'ont pas encore d'image. |
| `.btn-outline-primary` | Le guide (§3) indique qu'il reste à redéfinir, mais c'est déjà fait dans `style.css` : la note du guide peut être retirée. |
| Hero de l'accueil | Le code ajoute `data-bs-theme="dark"` sur le hero (focus blanc sur la photo), ce que le modèle du guide (§7.2) ne mentionne pas. Il faut l'ajouter au guide. |
