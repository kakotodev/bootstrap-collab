# VoyageAventure

Site vitrine d'une agence de voyages fictive, **VoyageAventure**. Il présente l'agence, ses destinations et un formulaire de contact.

Le projet consiste à **moderniser** ce site en HTML/CSS avec **Bootstrap 5.3** : design responsive (mobile-first), accessibilité et code plus simple à maintenir.

---

## Pages

| Page | Fichier | Contenu |
|---|---|---|
| Accueil | [index.html](index.html) | Présentation, services, témoignages |
| Destinations | [destinations.html](destinations.html) | Liste des voyages avec filtres par continent |
| À propos | [apropos.html](apropos.html) | Histoire, valeurs, équipe, chiffres clés |
| Contact | [contact.html](contact.html) | Coordonnées et formulaire de demande |

## Technologies

- HTML5 et CSS3
- [Bootstrap 5.3.8](https://getbootstrap.com/docs/5.3/) et [Bootstrap Icons 1.13.1](https://icons.getbootstrap.com), chargés par CDN
- Aucun outil de build : les fichiers s'ouvrent directement dans le navigateur

## Lancer le projet

1. Cloner le dépôt.
2. Ouvrir `index.html` dans un navigateur, ou utiliser l'extension **Live Server** de VS Code pour que la page se recharge à chaque modification.

Une connexion internet est nécessaire pour charger Bootstrap depuis le CDN.

## Structure

```
index.html            Accueil
destinations.html     Destinations
apropos.html          À propos
contact.html          Contact
css/style.css         Thème Bootstrap et styles propres au site
img/                  Images (favicon, photos)
GUIDE-STYLE.md        Charte graphique et composants à utiliser
```

## Contribuer

- **Avant de construire ou de modifier une page, lire le [guide de style](GUIDE-STYLE.md).** Il définit les couleurs, la typographie, l'ordre des sections et les composants Bootstrap de chaque page, pour que toutes les pages restent coordonnées.
- L'en-tête, le pied de page et le `<head>` sont communs aux 4 pages : toute modification de ces blocs doit être reportée sur toutes les pages.

## Avancement

- [x] Socle commun : Bootstrap par CDN, barre de navigation avec menu burger, pied de page, thème, favicon, balises SEO
- [ ] Accueil
- [ ] Contact (formulaire fonctionnel et conforme au RGPD)
- [ ] Destinations (filtres, fiches détaillées)
- [ ] À propos
- [ ] Pages Mentions légales et Politique de confidentialité
- [ ] Vraies photos (hero, destinations, équipe)

## Objectifs pédagogiques

- Mettre en page avec Flexbox, Grid et la grille Bootstrap
- Utiliser les composants Bootstrap en limitant les classes CSS personnalisées
- Écrire un HTML sémantique et accessible
- Concevoir en mobile-first
- Commenter le code HTML et CSS
