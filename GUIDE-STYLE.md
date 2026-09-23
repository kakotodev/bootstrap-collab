# Guide de style — VoyageAventure

Ce guide définit **à l'avance** l'apparence du site et les composants Bootstrap à utiliser dans chaque section.
Objectif : quelle que soit la personne qui construit une page, toutes les pages doivent être coordonnées (mêmes couleurs, mêmes titres, mêmes espacements, mêmes composants).

> Avant de construire ou de modifier une page, lire ce guide. En cas de doute, reproduire ce qui existe déjà sur une page terminée.

---

## Sommaire

1. [Principes](#1-principes)
2. [Installation (CDN)](#2-installation-cdn)
3. [Couleurs](#3-couleurs)
4. [Typographie](#4-typographie)
5. [Espacements et rythme des sections](#5-espacements-et-rythme-des-sections)
6. [Images et icônes](#6-images-et-icônes)
7. [Gabarits communs](#7-gabarits-communs)
8. [Composants](#8-composants)
9. [Plan de chaque page](#9-plan-de-chaque-page)
10. [Utilitaires Bootstrap courants](#10-utilitaires-bootstrap-courants)
11. [Check-list avant de terminer une page](#11-check-list-avant-de-terminer-une-page)

---

## 1. Principes

| Règle | En pratique |
|---|---|
| **Bootstrap d'abord** | Avant d'écrire du CSS, chercher la classe Bootstrap équivalente (grille, espacements, couleurs, typographie). |
| **Limiter les classes perso** | Seules les classes perso de la liste ci-dessous sont autorisées. Pour en ajouter une, en discuter avec l'équipe, puis la commenter dans `style.css` et l'ajouter à la liste. |
| **Mobile-first** | Écrire la version mobile par défaut, puis ajouter les variantes `-md-` et `-lg-` (`col-12 col-md-6`, `row-cols-1 row-cols-md-3`). Aucune largeur fixe en pixels. |
| **Pas de couleur en dur** | Utiliser les classes (`text-primary`, `bg-dark`…) ou les variables (`var(--bs-primary)`). |
| **Pas de `!important`** | Si une règle ne s'applique pas, revoir le sélecteur ou l'ordre de chargement. |
| **Pas de style en ligne** | Pas d'attribut `style="…"` dans le HTML. |
| **Commentaires** | Un commentaire HTML au début de chaque section : `<!-- Services -->`, `<!-- Témoignages -->`… |
| **Liens ≠ boutons** | Aller vers une autre page = `<a class="btn">`. Déclencher une action sur la page = `<button type="button">`. |

**Classes perso autorisées** (dans [css/style.css](css/style.css)) :

| Classe | Rôle |
|---|---|
| `.lien-evitement` | Position du lien « Aller au contenu » quand il reçoit le focus |
| `.hero` | Photo de fond du hero de l'accueil |
| Règles `:has()` des filtres | Masquer les destinations non sélectionnées (voir §8.5) |

---

## 2. Installation (CDN)

Versions utilisées : **Bootstrap 5.3.8** et **Bootstrap Icons 1.13.1**. Ne pas changer de version sans mettre à jour les empreintes `integrity`.

Dans le `<head>`, dans cet ordre :

```html
<!-- Bootstrap 5.3 et Bootstrap Icons (CDN) -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/css/bootstrap.min.css"
    integrity="sha384-sRIl4kxILFvY47J16cr9ZwB07vP4J8+LH7qKQnuqkuIAvNWLzeN8tE5YBujZqJLB" crossorigin="anonymous">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.13.1/font/bootstrap-icons.min.css"
    integrity="sha384-CK2SzKma4jA5H/MXDUU7i1TqZlCFaD4T01vtyDFvPlD97JQyS+IsSh1nI2EFbpyk" crossorigin="anonymous">

<!-- Styles du site : chargés après Bootstrap pour pouvoir le personnaliser -->
<link rel="stylesheet" href="css/style.css">
```

Juste avant `</body>` :

```html
<!-- JavaScript de Bootstrap (menu burger, modales) -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-FKyoEForCGlyvwx9Hj09JcYn3nv7wiPVlz7YYwJrWVcXK/BmnVDxM+D2scQbITxI"
    crossorigin="anonymous"></script>
```

---

## 3. Couleurs

Le thème est défini **une seule fois**, dans le bloc `:root` en haut de [css/style.css](css/style.css).

| Rôle | Variable / classes | Valeur | Où l'utiliser |
|---|---|---|---|
| Principal | `--bs-primary` · `btn-primary`, `text-primary` | `#1d6fa5` | Boutons d'action, liens, icônes, éléments actifs |
| Marque (bleu nuit) | `--bs-dark` · `bg-dark` | `#2c3e50` | En-tête, pied de page, bandeaux de titre, chiffres clés |
| Texte | `--bs-body-color` | `#333` | Texte courant |
| Texte secondaire | `text-body-secondary` | — | Descriptions, sous-titres, légendes |
| Fond blanc | `bg-white` | `#fff` | Sections paires (voir §5) |
| Fond gris clair | `bg-body-tertiary` | `#f8f9fa` | Sections impaires (voir §5) |

**Interdits** : rouge, vert ou violet décoratifs, et les couleurs d'état (`success`, `danger`, `warning`) utilisées hors des messages d'alerte du formulaire. **Une seule couleur d'action sur tout le site : `primary`.**

**Zones sombres** : toujours ajouter `data-bs-theme="dark"` sur un conteneur `bg-dark`. Les textes, liens et contours de focus passent alors automatiquement en clair.

> Les variantes de boutons ne suivent pas automatiquement `--bs-primary` quand Bootstrap est chargé par CDN. `.btn-primary` est déjà redéfini dans `style.css`. Il faudra faire de même pour `.btn-outline-primary` quand les filtres seront construits.

---

## 4. Typographie

Police : la **pile système de Bootstrap** (pas de Google Fonts pour l'instant). Interligne : 1,6.

| Élément | Balise | Classes |
|---|---|---|
| Titre du hero (accueil) | `<h1>` | `display-4 fw-bold` |
| Titre du bandeau (pages intérieures) | `<h1>` | `display-5 fw-bold` |
| Sous-titre du hero ou du bandeau | `<p>` | `lead` (+ `text-body-secondary` dans le bandeau) |
| Titre de section | `<h2>` | `fw-bold text-center mb-5` |
| Titre de carte | `<h3>` | `card-title h5` |
| Titres du pied de page | `<h2>` | `h5` |
| Chiffre clé | `<dd>` | `display-5 fw-bold` |
| Texte courant | `<p>` | aucune classe |

Règles :
- **Un seul `<h1>` par page**, et pas de niveau sauté (h1 → h2 → h3). La balise indique le niveau du titre, la classe indique sa taille.
- **Typographie française** : espace insécable avant `€ : ; ! ?` (`&nbsp;`), guillemets « » pour les citations, heures au format « 9 h – 18 h ».

---

## 5. Espacements et rythme des sections

- Chaque section : `<section class="py-5 …">` + `<div class="container">`.
- Entre les cartes d'une grille : `g-4`. Entre deux colonnes de mise en page : `g-5`.
- Sous un titre de section : `mb-5`.
- **Alternance des fonds** : après le bandeau ou le hero, les sections alternent `bg-white` puis `bg-body-tertiary`. On ne place jamais deux sections de même fond l'une après l'autre.
- `bg-dark` est réservé à l'en-tête, au pied de page, aux bandeaux de titre et aux chiffres clés. Une section `bg-dark` n'est jamais placée juste avant le pied de page.

Arrondis et ombres : rayon par défaut de Bootstrap partout. `shadow-sm` sur les cartes uniquement. `rounded-pill` seulement pour les filtres.

---

## 6. Images et icônes

### Images

| Usage | Dimensions | Ratio | Fichier |
|---|---|---|---|
| Hero de l'accueil | 1920 × 1080 | 16:9 | `img/hero.webp` |
| Carte de destination | 600 × 400 | 3:2 | `img/destinations/<ville>.webp` (ex. `rome.webp`) |
| Photo « Notre histoire » | 800 × 600 | 4:3 | `img/apropos/histoire.webp` |
| Portrait de l'équipe | 240 × 240 | 1:1 | `img/equipe/<prenom-nom>.webp` |

Règles :
- Format **WebP**, noms de fichiers en minuscules, sans accent ni espace.
- Toujours renseigner `width`, `height` et `alt`, plus `loading="lazy"` pour toutes les images sauf celle du hero.
- `alt` décrit ce que montre l'image (« Le Colisée de Rome au coucher du soleil »), et non « image de Rome ».

### Icônes

Uniquement **Bootstrap Icons**, toujours avec `aria-hidden="true"`. **Plus aucun emoji.**

| Emplacement | Icône | Taille / couleur |
|---|---|---|
| Logo | `bi-globe-americas` | taille du texte |
| Services : Voyages organisés / Réservations / Sur mesure | `bi-airplane` / `bi-building` / `bi-backpack` | `fs-1 text-primary` |
| Valeurs : Excellence / Confiance / Respect | `bi-star` / `bi-shield-check` / `bi-globe-europe-africa` | `fs-1 text-primary` |
| Contact : Adresse / Téléphone / E-mail / Horaires | `bi-geo-alt` / `bi-telephone` / `bi-envelope` / `bi-clock` | `fs-4 text-primary` (page Contact), `me-2` (pied de page) |
| Durée d'un voyage | `bi-calendar3` | dans le `badge` |
| Lien « Voir plus » | `bi-arrow-right` | après le texte, `ms-1` |

---

## 7. Gabarits communs

### 7.1 Squelette de page

L'en-tête et le pied de page sont **déjà en place et identiques sur les 4 pages** : il ne faut pas les modifier dans une seule page. Le contenu de la page se place entre eux, dans `<main>` :

```html
<body>
    <!-- Lien d'évitement … -->
    <!-- En-tête … (commun) -->

    <!-- Contenu principal -->
    <main id="contenu">
        <!-- Bandeau de titre (ou Hero sur l'accueil) -->
        <!-- Sections de la page, en alternant les fonds -->
    </main>

    <!-- Pied de page … (commun) -->
    <!-- JavaScript de Bootstrap -->
</body>
```

Par page, seuls changent : le `<title>`, la `meta description`, `og:title` / `og:description` et le lien actif du menu (`class="nav-link active" aria-current="page"`).

### 7.2 Hero (accueil uniquement)

```html
<!-- Hero -->
<section class="hero text-white text-center py-5">
    <div class="container py-5">
        <h1 class="display-4 fw-bold">Explorez le monde avec nous</h1>
        <p class="lead mb-4">Découvrez des destinations extraordinaires et créez des souvenirs inoubliables.</p>
        <div class="d-flex flex-column flex-sm-row justify-content-center gap-3">
            <a href="destinations.html" class="btn btn-primary btn-lg">Découvrir nos voyages</a>
            <a href="contact.html" class="btn btn-outline-light btn-lg">Nous contacter</a>
        </div>
    </div>
</section>
```

```css
/* Hero : photo de fond assombrie pour garder le texte blanc lisible */
.hero {
    background: linear-gradient(rgba(0, 0, 0, 0.55), rgba(0, 0, 0, 0.55)), url("../img/hero.webp") center / cover;
}
```

### 7.3 Bandeau de titre (Destinations, À propos, Contact)

```html
<!-- Bandeau de titre -->
<section class="bg-dark text-white text-center py-5" data-bs-theme="dark">
    <div class="container">
        <h1 class="display-5 fw-bold">Nos destinations</h1>
        <p class="lead text-body-secondary mb-0">Explorez les merveilles du monde</p>
    </div>
</section>
```

### 7.4 Section standard

```html
<!-- Nom de la section -->
<section class="py-5 bg-white">
    <div class="container">
        <h2 class="fw-bold text-center mb-5">Titre de la section</h2>
        <div class="row row-cols-1 row-cols-md-3 g-4">
            <div class="col">…carte…</div>
        </div>
    </div>
</section>
```

---

## 8. Composants

### 8.1 Grilles

Listes de cartes : `row row-cols-*` + `g-4`, jamais de `width` fixe.

| Contenu | Grille |
|---|---|
| Services, valeurs, équipe | `row-cols-1 row-cols-md-3` |
| Destinations | `row-cols-1 row-cols-sm-2 row-cols-lg-3` |
| Témoignages | `row-cols-1 row-cols-md-2` |
| Chiffres clés | `row-cols-2 row-cols-md-4` |
| Deux colonnes (texte + image, coordonnées + formulaire) | `row g-5` + `col-lg-7` / `col-lg-5` |

### 8.2 Boutons

| Usage | Classes |
|---|---|
| Action principale | `btn btn-primary` (`btn-lg` dans le hero et le formulaire) |
| Action secondaire sur fond sombre ou photo | `btn btn-outline-light` |
| Filtre | `btn btn-outline-primary rounded-pill` |
| Fermer une modale | `btn-close` |

### 8.3 Carte avec icône (services, valeurs)

```html
<div class="col">
    <article class="card h-100 shadow-sm border-0 text-center p-4">
        <i class="bi bi-airplane fs-1 text-primary" aria-hidden="true"></i>
        <div class="card-body">
            <h3 class="card-title h5">Voyages organisés</h3>
            <p class="card-text text-body-secondary">Des circuits complets avec guide professionnel.</p>
        </div>
    </article>
</div>
```

### 8.4 Carte de destination

```html
<div class="col" data-continent="europe">
    <article class="card h-100 shadow-sm border-0">
        <img src="img/destinations/rome.webp" class="card-img-top" alt="Le Colisée de Rome au coucher du soleil"
            width="600" height="400" loading="lazy">
        <div class="card-body">
            <span class="badge text-bg-light mb-2"><i class="bi bi-calendar3 me-1" aria-hidden="true"></i>6 jours</span>
            <h3 class="card-title h5">Rome, Italie</h3>
            <p class="card-text text-body-secondary">La ville éternelle</p>
        </div>
        <div class="card-footer bg-transparent border-0 d-flex justify-content-between align-items-center pb-3">
            <span class="fw-bold">À partir de 499&nbsp;€</span>
            <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#detail-rome">
                Voir détails
            </button>
        </div>
    </article>
</div>
```

`data-continent` se met sur la **colonne** (`.col`), pour que le filtre masque la colonne entière.

### 8.5 Filtres des destinations : `btn-check` + `:has()` (sans JS)

```html
<!-- Filtres -->
<fieldset class="text-center mb-5">
    <legend class="h6">Filtrer par continent</legend>
    <div class="d-flex flex-wrap justify-content-center gap-2">
        <input type="radio" class="btn-check" name="continent" id="f-tous" checked>
        <label class="btn btn-outline-primary rounded-pill" for="f-tous">Tous</label>

        <input type="radio" class="btn-check" name="continent" id="f-europe">
        <label class="btn btn-outline-primary rounded-pill" for="f-europe">Europe</label>
        <!-- … un couple input/label par continent : asie, amerique, afrique, oceanie … -->
    </div>
</fieldset>
```

```css
/* Filtres : masque les destinations qui ne correspondent pas au continent coché */
.destinations:has(#f-europe:checked) [data-continent]:not([data-continent="europe"]),
.destinations:has(#f-asie:checked) [data-continent]:not([data-continent="asie"]) {
    display: none;
}
```

Identifiants des continents (les mêmes partout, y compris dans le formulaire de contact) : `europe`, `asie`, `amerique`, `afrique`, `oceanie`.

### 8.6 Modale « Voir détails »

Un seul modèle pour toutes les destinations. L'`id` suit la forme `detail-<ville>`.

```html
<div class="modal fade" id="detail-rome" tabindex="-1" aria-labelledby="detail-rome-titre" aria-hidden="true">
    <div class="modal-dialog modal-lg modal-dialog-centered modal-dialog-scrollable">
        <div class="modal-content">
            <div class="modal-header">
                <h2 class="modal-title h5" id="detail-rome-titre">Rome, Italie</h2>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Fermer"></button>
            </div>
            <div class="modal-body">
                <!-- Programme : list-group, un élément par jour -->
            </div>
            <div class="modal-footer">
                <a href="contact.html" class="btn btn-primary">Demander un devis</a>
            </div>
        </div>
    </div>
</div>
```

### 8.7 Témoignage

```html
<div class="col">
    <figure class="card h-100 shadow-sm border-0 p-4 mb-0">
        <blockquote class="blockquote mb-3">
            <p>« Un voyage fantastique en Asie ! L'équipe a été aux petits soins. »</p>
        </blockquote>
        <figcaption class="blockquote-footer mb-0">Marie D., <cite>circuit Japon, 2025</cite></figcaption>
    </figure>
</div>
```

### 8.8 Membre de l'équipe

```html
<div class="col">
    <article class="text-center">
        <img src="img/equipe/jean-dupont.webp" class="rounded-circle mb-3" alt="Portrait de Jean Dupont"
            width="120" height="120" loading="lazy">
        <h3 class="h5 mb-1">Jean Dupont</h3>
        <p class="text-primary fw-semibold mb-1">Directeur général</p>
        <p class="text-body-secondary">15 ans d'expérience dans le tourisme</p>
    </article>
</div>
```

### 8.9 Chiffres clés

Une liste de définitions : le lecteur d'écran lit « Voyages organisés : 5000+ ». `flex-column-reverse` affiche le chiffre au-dessus du libellé.

```html
<!-- Chiffres clés -->
<section class="bg-dark text-white py-5" data-bs-theme="dark">
    <div class="container">
        <h2 class="fw-bold text-center mb-5">VoyageAventure en chiffres</h2>
        <dl class="row row-cols-2 row-cols-md-4 g-4 text-center mb-0">
            <div class="col d-flex flex-column-reverse">
                <dt class="fw-normal text-body-secondary">Voyages organisés</dt>
                <dd class="display-5 fw-bold mb-1">5000+</dd>
            </div>
            <!-- … -->
        </dl>
    </div>
</section>
```

### 8.10 Coordonnées (page Contact)

```html
<address>
    <ul class="list-unstyled">
        <li class="d-flex gap-3 mb-4">
            <i class="bi bi-telephone fs-4 text-primary" aria-hidden="true"></i>
            <div>
                <h3 class="h6 mb-1">Téléphone</h3>
                <a href="tel:+33123456789">01 23 45 67 89</a>
            </div>
        </li>
        <!-- Adresse, E-mail (mailto:), Horaires : même structure -->
    </ul>
</address>
```

### 8.11 Formulaire

| Élément | Classes / attributs |
|---|---|
| Conteneur | `card shadow-sm border-0 p-4` |
| Texte, e-mail, téléphone | `form-floating` > `form-control` + `<label>` (le label vient **après** l'input, avec un `placeholder`) |
| Liste déroulante | `form-floating` > `form-select` + `<label>` |
| Message | `form-floating` > `<textarea class="form-control">` (hauteur fixée par `rows`) |
| Case à cocher | `form-check` > `form-check-input` + `form-check-label` |
| Validation | `novalidate` + `needs-validation` sur le `<form>`, un `invalid-feedback` sous chaque champ obligatoire |
| Message après envoi | `alert alert-success` ou `alert alert-danger` |
| Mention RGPD | `form-text` sous le bouton |

```html
<form class="needs-validation" action="https://formspree.io/f/VOTRE_ID" method="POST" novalidate>
    <div class="form-floating mb-3">
        <input type="email" class="form-control" id="email" name="email" placeholder="nom@exemple.fr"
            autocomplete="email" required>
        <label for="email">Adresse e-mail</label>
        <div class="invalid-feedback">Indiquez une adresse e-mail valide.</div>
    </div>

    <!-- Consentement marketing : FACULTATIF (RGPD) -->
    <div class="form-check mb-3">
        <input class="form-check-input" type="checkbox" id="newsletter" name="newsletter">
        <label class="form-check-label" for="newsletter">Je souhaite recevoir vos offres par e-mail</label>
    </div>

    <button type="submit" class="btn btn-primary btn-lg">Envoyer le message</button>
    <p class="form-text mt-3 mb-0">Vos données servent uniquement à répondre à votre demande.
        <a href="confidentialite.html">Politique de confidentialité</a></p>
</form>
```

`VOTRE_ID` est à remplacer une fois le compte Formspree créé.

Le script de validation est la seule exception au « sans JS ». On reprend celui de la documentation Bootstrap, placé après le script Bootstrap :

```html
<!-- Validation du formulaire (script de la documentation Bootstrap) -->
<script>
    document.querySelectorAll('.needs-validation').forEach(form => {
        form.addEventListener('submit', event => {
            if (!form.checkValidity()) {
                event.preventDefault();
                event.stopPropagation();
            }
            form.classList.add('was-validated');
        });
    });
</script>
```

### 8.12 Composants réservés aux évolutions

| Composant | Usage prévu |
|---|---|
| `accordion` | FAQ (« Faut-il un visa ? », « Comment payer ? »…) |
| `carousel` | Galerie photo dans une modale de destination |
| `list-group` | Programme jour par jour dans une modale |
| `ratio ratio-16x9` | Carte de l'agence (iframe) sur la page Contact |

Aucun autre composant Bootstrap sans accord de l'équipe : c'est ce qui garde le site cohérent.

---

## 9. Plan de chaque page

Chaque tableau donne l'ordre des sections, de haut en bas.

### Accueil — `index.html`

| # | Section | Fond | Composants |
|---|---|---|---|
| 1 | Hero | photo (`.hero`) | §7.2 : `display-4`, `lead`, `btn-primary` + `btn-outline-light` |
| 2 | Nos services | `bg-white` | §8.3 × 3, grille `row-cols-md-3` |
| 3 | Ils nous font confiance | `bg-body-tertiary` | §8.7 × 2, grille `row-cols-md-2` |

### Destinations — `destinations.html`

| # | Section | Fond | Composants |
|---|---|---|---|
| 1 | Bandeau « Nos destinations » | `bg-dark` | §7.3 |
| 2 | Filtres + liste (`<section class="destinations">`) | `bg-body-tertiary` | §8.5 filtres, §8.4 cartes, grille `row-cols-sm-2 row-cols-lg-3` |
| — | Modales (une par destination) | — | §8.6, placées à la fin de `<main>` |

### À propos — `apropos.html`

| # | Section | Fond | Composants |
|---|---|---|---|
| 1 | Bandeau « À propos de nous » | `bg-dark` | §7.3 |
| 2 | Notre histoire | `bg-white` | `row g-5 align-items-center` : texte `col-lg-7`, photo `col-lg-5` (`img-fluid rounded`) |
| 3 | Chiffres clés | `bg-dark` | §8.9 |
| 4 | Nos valeurs | `bg-body-tertiary` | §8.3 × 3, grille `row-cols-md-3` |
| 5 | Notre équipe | `bg-white` | §8.8 × 3, grille `row-cols-md-3` |

Les chiffres clés passent juste après « Notre histoire », pour ne pas placer une section sombre contre le pied de page (§5).

### Contact — `contact.html`

| # | Section | Fond | Composants |
|---|---|---|---|
| 1 | Bandeau « Contactez-nous » | `bg-dark` | §7.3 |
| 2 | Coordonnées + formulaire | `bg-body-tertiary` | `row g-5` : coordonnées `col-lg-5` (§8.10), formulaire `col-lg-7` (§8.11) |

### Pages légales (à créer) — `mentions-legales.html`, `confidentialite.html`

| # | Section | Fond | Composants |
|---|---|---|---|
| 1 | Bandeau | `bg-dark` | §7.3 |
| 2 | Texte | `bg-white` | `row justify-content-center` > `col-lg-8`, titres `<h2 class="h4">` |

---

## 10. Utilitaires Bootstrap courants

À utiliser à la place du CSS perso :

| Besoin | Classes |
|---|---|
| Espacement vertical des sections | `py-5` |
| Marges internes / externes | `p-*`, `m-*`, `mb-*`, `mt-*`, `gap-*` (échelle de 0 à 5) |
| Alignement | `text-center`, `d-flex`, `justify-content-*`, `align-items-*` |
| Taille d'un titre sans changer son niveau | `h1` … `h6`, `display-*` |
| Texte | `lead`, `fw-bold`, `fw-semibold`, `small`, `text-body-secondary` |
| Images | `img-fluid`, `rounded`, `rounded-circle`, `object-fit-cover` |
| Masquer visuellement (lecteurs d'écran) | `visually-hidden`, `visually-hidden-focusable` |

---

## 11. Check-list avant de terminer une page

- [ ] Les sections suivent l'ordre et les fonds du §9, avec un commentaire HTML au début de chacune.
- [ ] Aucune classe perso hors de la liste du §1, aucun `style="…"`, aucun `!important`, aucun emoji.
- [ ] Un seul `<h1>`, des titres sans niveau sauté, les classes de titre du §4.
- [ ] Chaque image a `alt`, `width` et `height`. Chaque icône a `aria-hidden="true"`.
- [ ] Chaque champ de formulaire a un `<label>` et un attribut `autocomplete` quand c'est pertinent.
- [ ] La page est utilisable entièrement au clavier, et le focus est toujours visible.
- [ ] Les contrastes font au moins 4,5:1 : pas de gris clair sur fond blanc, utiliser `text-body-secondary`.
- [ ] Pas de défilement horizontal à 360 px de large. La page a été vérifiée à 360 px, à 768 px et à 1280 px.
- [ ] La typographie française est respectée (§4).
