# ⬡ LiDAR Annotate

<div align="center">

**Plateforme collaborative d'annotation de nuages de points 3D**  
*Conçue pour la détection d'obstacles électriques en vol basse altitude — Airbus Helicopters*

[![Demo](https://img.shields.io/badge/🚀_Demo_Live-GitHub_Pages-00d4ff?style=for-the-badge)](https://votre-username.github.io/lidar-annotate)
[![License](https://img.shields.io/badge/License-MIT-7c6bff?style=for-the-badge)](LICENSE)
[![Made with](https://img.shields.io/badge/Vanilla_JS_+_Three.js-00e676?style=for-the-badge)](#stack)
[![Context](https://img.shields.io/badge/Airbus_Helicopters_×_SOREAM-ffb300?style=for-the-badge)](#contexte)

</div>

---

<div align="center">

```
  Charger PCD  →  Annoter points  →  Générer OBB  →  Export CVAT
      📁              🖌️                 ⬡                 📦
  ASCII/binaire    Multi-classes      PCA 3D fit       Supervisely 1.0
```

</div>

---

## 🎯 Contexte

Développé lors d'un stage de 12 semaines au sein de l'équipe **ETRV d'Airbus Helicopters** (Marignane) via **SOREAM** — dans le cadre d'un projet de recherche sur la **détection automatique d'obstacles électriques** (câbles, pylônes, antennes, éoliennes) pour hélicoptères en vol basse altitude.

L'outil a permis de **préparer les données LiDAR** pour entraîner un modèle de deep learning, en remplacement de CVAT pour les nuages de points lourds. Il a contribué directement à une **démonstration officielle avec l'armée française**.

---

## ✨ Fonctionnalités

### 🖥️ Dashboard collaboratif (style CVAT)
- Gestion de **projets**, **tâches** et **équipes** avec rôles (Admin / Annotateur / Relecteur)
- **Kanban board** : À faire → En cours → En revue → Validé → Rejeté
- Statistiques de progression en temps réel, historique d'activité
- Données persistées en `localStorage` — **zéro backend, zéro dépendance**

### ⬡ Annotateur 3D LiDAR
| Fonctionnalité | Détails |
|---|---|
| **Formats supportés** | `.pcd` ASCII, `.xyz`, `.txt`, `.json`, `.zip` Supervisely |
| **Modes d'annotation** | Rotation · Peindre · Effacer · Pinceau (rayon + densité) |
| **Classes & instances** | 4 types fixes + instances illimitées par type (Cable #1, #2...) |
| **Navigation multi-frames** | 100+ PCD, lazy parsing, vue caméra persistante entre frames |
| **Génération OBB** | PCA 3D, détection pylône vs câble, axes exacts A→B |
| **Raccourcis configurables** | Par action globale ET par instance de classe |
| **Color picker RGB** | Sliders R/G/B + HEX + palette + nouveau type de classe |

### 📦 Export
- **Supervisely Point Cloud 1.0** — ZIP compatible CVAT direct (`ds0/ann/`, `meta.json`, `key_id_map.json`)
- **JSON** annotations avec coordonnées absolues + métadonnées classes
- **PCD labelisé** avec champ `label` intégré (compatible Open3D)
- **Multi-frames** : toutes les frames annotées en un seul export ZIP

---

## 🧠 Algorithme OBB (Oriented Bounding Box)

```
1. Trouver les 2 points les plus éloignés (diamètre A→B)
2. Détecter la nature : pylône (vertical > 70%) vs câble (horizontal)
3. Construire le repère orthonormé :
     ex = vnorm(A→B)                  ← axe long
     ey = vnorm([-ex.y, ex.x, 0])     ← perpendiculaire horizontal
     ez = cross(ex, ey)               ← hauteur
4. Projeter tous les points → vrai min/max par axe
5. Exporter position + rotation Euler + dimensions en coords absolues
```

R�sultat : des cuboïdes qui épousent exactement la géométrie des câbles électriques, sans débordement sur les structures voisines.

---

## 🚀 Démarrage rapide

### Utilisation locale (aucune installation)
```bash
git clone https://github.com/votre-username/lidar-annotate.git
cd lidar-annotate
open index.html        # macOS
start index.html       # Windows
xdg-open index.html    # Linux
```

### Déploiement équipe sur GitHub Pages
```
1. Fork ce repo
2. Settings → Pages → Source: main branch → Save
3. Partager l'URL : https://votre-username.github.io/lidar-annotate
```

> ⚡ **Aucune dépendance à installer.** Three.js et JSZip sont chargés via CDN.

---

## 🗂️ Structure du projet

```
lidar-annotate/
├── index.html          # Dashboard plateforme (62 KB)
│   ├── Gestion projets & tâches
│   ├── Kanban board
│   ├── Gestion équipe (rôles, invitations)
│   └── Import / Export datasets
│
└── annotator.html      # Annotateur 3D standalone (128 KB)
    ├── Renderer Three.js r128 (WebGL)
    ├── Parser PCD ASCII (chunked async)
    ├── Système OBB / PCA (Jacobi eigendecomposition)
    ├── Multi-frames avec lazy loading
    └── Export Supervisely ZIP (client-side pur)
```

---

## 🛠️ Stack technique

| Couche | Technologies |
|---|---|
| **Rendu 3D** | Three.js r128 — WebGL, PointsMaterial, EdgesGeometry |
| **Algorithmes** | PCA (Jacobi), OBB par diamètre convexe, projection 3D→2D |
| **Formats** | PCD ASCII, Supervisely Point Cloud 1.0, CVAT-compatible |
| **UI** | Vanilla JS ES6+, CSS custom properties, JetBrains Mono |
| **Persistance** | localStorage (dashboard) + session memory (annotateur) |
| **Export** | JSZip 3.10 — ZIP 100% client-side |
| **Déploiement** | GitHub Pages — **0 serveur · 0 backend · 0 framework** |

---

## 📊 Performances

| Métrique | Valeur |
|---|---|
| Parsing PCD 50k pts | ~400ms (async chunked) |
| Rendu 500k pts | 60 FPS stable |
| Taille totale projet | **190 KB** (deux fichiers HTML) |
| Dépendances runtime | 2 (Three.js + JSZip, CDN) |
| Installation | **Aucune** |

---

## 🏗️ Architecture annotateur

```
┌──────────────────────────────────────────────────────┐
│                   LiDAR Annotator                    │
├──────────────┬───────────────────┬───────────────────┤
│  PCD Parser  │   3D Engine       │  Annotation       │
│  (async)     │   (Three.js)      │  System           │
│              │                   │                   │
│  *.pcd       │  WebGL renderer   │  imap Int32Array  │
│  *.xyz       │  Orbit camera     │  OBB / PCA fit    │
│  *.zip Sly   │  Point cloud      │  Multi-class      │
│              │  Cuboid meshes    │  Instance mgmt    │
└──────┬───────┴────────┬──────────┴──────────┬────────┘
       │                │                     │
       ▼                ▼                     ▼
  Frame Manager    Camera state          Supervisely
  (lazy loading)   (persistent)          ZIP Export
```

---

## 🎬 Workflow d'annotation

```
📁 Importer 100+ frames PCD
        ↓
🖌️  Peindre les points par instance
    (Cable #1, Pylone #1, Antenne #1...)
        ↓
⬡  Générer les cuboïdes OBB
   (automatique sur toutes les frames annotées)
        ↓
✏️  Affiner manuellement si besoin
    (éditeur position / rotation / dimensions)
        ↓
📦 Exporter ZIP Supervisely → Import CVAT ✅
```

---

## 📄 Licence

MIT © 2026 Arthur Duret  
Développé dans le cadre d'un stage chez **Airbus Helicopters** (Marignane) via **SOREAM**

---

<div align="center">

**Three.js · Vanilla JS · Zéro backend · Zéro framework · 190 KB**

*Si ce projet t'a été utile, une ⭐ est appréciée !*

</div>
