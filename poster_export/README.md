# 📋 Poster de stage — Airbus Helicopters × SOREAM

Poster de synthèse du stage de 12 semaines réalisé au sein de l'équipe **ETRV d'Airbus Helicopters** (Marignane), via **SOREAM**, sur la **préparation de données LiDAR et vidéo** pour l'entraînement d'un modèle de détection d'obstacles aéronautiques (câbles, pylônes électriques) en vol à basse altitude.

## Contenu

- `index.html` — structure du poster (header, 12 sections, overlay de zoom interactif, navigation clavier)
- `styles.css` — feuille de style complète (thème dark/blue, cartes, tableaux Gantt, SVG pipeline)

## Aperçu

Le poster couvre :
1. Présentation Airbus Helicopters & service ETRV
2. Contexte, enjeux et politiques entreprise
3. Définition de la mission
4. Pipeline d'entraînement IA (schéma SVG)
5. Planning réel (Gantt 12 semaines)
6. Détail des phases du stage
7. Méthodes & choix techniques
8. Visuels d'annotation
9. Bilan objectifs vs résultats
10. Difficultés rencontrées & solutions
11. Impact & projection professionnelle
12. Démonstration officielle avec l'armée française
13. Conclusion

## Utilisation

Ouvrir `index.html` dans un navigateur (double-clic ou via un serveur local) :

```bash
cd poster_export
python3 -m http.server 8000
# puis ouvrir http://localhost:8000
```

Cliquer sur une carte zoome dessus (navigation clavier : flèches / espace / échap).

> Les images visuelles (`airbus-logo-blanc.png`, `soream-logo-blanc.png`, `cesi-logo-blanc.png`, captures d'annotation) ne sont pas incluses — un placeholder s'affiche automatiquement si absentes.

## Stack

HTML5 + CSS3 (custom properties) + Vanilla JS — zéro dépendance, zéro build.
