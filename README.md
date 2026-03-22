# 🐜 TP NSI — Algorithme de Colonies de Fourmis (Ant System)

> Ressource pédagogique interactive pour la **Terminale NSI**  
> Lycée Antoine Watteau — DarkSATHI Li  
> Générée avec Claude · Prompt engineering documenté

---

## Aperçu

Ce dépôt contient une page HTML **entièrement autonome** (zéro serveur requis)
qui guide les élèves de Terminale NSI à travers la découverte et l'implémentation
de l'algorithme **Ant System** de Marco Dorigo (1992).

Le TP s'ouvre directement dans un navigateur moderne depuis un bureau, une clé USB
ou un ENT. Aucune installation n'est nécessaire pour les exercices en ligne.

---

## Contenu du dépôt

```
.
├── tp_ant_system.html          ← Le TP complet (page autonome, ~700 Ko avec image)
├── prompt_exemplaire.md        ← Le prompt Claude qui reproduit ce résultat
└── README.md                   ← Ce fichier
```

---

## Ce que fait ce TP

### 9 sections progressives

| # | Section | Type | Capacité BO |
|---|---------|------|-------------|
| 1 | Introduction + Animation JS | Découverte | — |
| 2 | Cours — Graphes et cycle hamiltonien | Cours | Graphes |
| 3 | Exercice — Stratégie gloutonne à la main | Exercice papier | Graphes |
| 4 | Cours — L'algorithme Ant System | Cours | Algorithmique |
| 4½ | Exercice — Calcul de probabilité à la main | Exercice calcul | Algorithmique |
| 5 | Exercice — Sac à dos 0/1 | Code PyScript | Prog. dynamique |
| 6 | Cours — Mémoïsation vs tabulaire | Cours | Prog. dynamique |
| 7 | Exercice — Implémenter l'Ant System | Code PyScript | POO |
| 8 | Projet — Pygame + SQLite | Projet guidé | POO + SQL |

### Fonctionnalités interactives

- **Animation Canvas JS** — simulation temps réel de l'algorithme sur 7 villes,
  avec phéromones en teinte et épaisseur variables, fourmis animées,
  courbe de convergence en direct, sliders (nb fourmis, taux d'évaporation)
- **Système de déblocage conditionnel** — chaque correction est verrouillée
  derrière une question à laquelle l'élève doit répondre correctement.
  Impossible de « sauter » une étape. Feedback différencié selon l'erreur.
  3 tentatives max, puis affichage automatique.
- **Éditeurs Python PyScript** — 5 éditeurs exécutables dans le navigateur,
  avec fonctions de test pré-écrites que l'élève doit faire passer
- **Barre de progression** — suit l'avancement via `IntersectionObserver`
- **Mode clair / sombre** — pour utilisation sur vidéoprojecteur ou écran personnel

### Couverture du Bulletin Officiel NSI Terminale

| Capacité exigible | Couverture dans le TP |
|-------------------|-----------------------|
| Modéliser avec un graphe | Cours 1, matrice d'adjacence, cycle hamiltonien |
| Programmation dynamique | Exercice sac à dos 0/1, tableau dp, reconstitution |
| POO | 4 classes : `Ville`, `Colonie`, `Visualiseur`, `BaseDonnees` |
| SQL | `CREATE TABLE`, `INSERT`, `SELECT…ORDER BY…LIMIT`, `DELETE…WHERE` |

---

## Prérequis élèves

- Python (récursivité, listes, dictionnaires, fonctions, POO)
- Graphes (notion de sommet, arête, vu en Première ou début Terminale)
- Programmation dynamique basique

> ⚠️ **Conçu pour des classes mixtes** — une partie des élèves n'a pas la
> spécialité mathématiques. Toutes les formules sont décomposées étape par étape
> avec des exemples numériques concrets. Les symboles Σ, ←, Δ, α, β, η, τ, ρ
> sont expliqués en français ordinaire.

---

## Utilisation

### En classe

1. Télécharger `tp_ant_system.html`
2. L'ouvrir dans Chrome, Firefox ou Edge (navigateur récent)
3. Aucune connexion internet requise pour les exercices PyScript une fois chargé

> 💡 Pour la partie projet finale (Pygame + SQLite),
> les élèves travaillent en Python local.
> Installation : `pip install pygame`

### En vidéoprojection

Basculer en **mode clair** (bouton en haut à droite) pour une meilleure
lisibilité sur fond blanc.

---

## Architecture technique

```
tp_ant_system.html
│
├── CSS (dans <head>)
│   ├── Bootstrap 5.3.3 (CDN)
│   ├── Google Fonts : Rubik Dirt / Source Serif 4 / JetBrains Mono
│   └── Charte DarkSATHI Li (variables CSS, dark theme)
│
├── HTML
│   ├── Image d'entête (base64 intégrée — aucun fichier externe)
│   ├── 9 sections de contenu
│   ├── 10 blocs de déblocage progressif
│   ├── 5 éditeurs PyScript (env="dynamique" partagé)
│   └── 2 canvas JS (animation fourmis + courbe de convergence)
│
└── JavaScript (en fin de <body>)
    ├── Moteur animation Ant System (fidèle à l'algo réel)
    ├── Système de déblocage (10 fonctions verifier*())
    ├── Barre de progression (IntersectionObserver)
    └── Canvas graphe statique avec tournée optimale
```

**CDN utilisés :**
- `bootstrap@5.3.3`
- `mathjax@3` (avec config `window.MathJax` avant chargement)
- `pyscript@2026.3.1`

**Dépendances tierces :** aucune (image en base64, JS vanilla)

---

## Choix pédagogiques notables

### Le système de déblocage conditionnel

C'est la décision architecturale centrale du TP. Contrairement à un TP classique
où l'élève peut consulter la correction à tout moment, chaque correction est
protégée par une question à laquelle il faut avoir répondu correctement.

La question porte toujours sur un **calcul intermédiaire** que l'élève
vient de faire (ou doit faire) dans l'exercice. Exemple : pour débloquer
la correction de l'exercice 1, il faut entrer la distance de la tournée
gloutonne (63 km). Pour débloquer l'indice du sac à dos, il faut entrer
la valeur optimale pour une capacité de 2 kg (7 pts).

Ce mécanisme transforme le TP passif (lire la correction) en activité
active (calculer quelque chose d'abord).

### L'exercice de calcul de probabilité à la main

Entre le cours sur la formule et l'exercice de programmation se glisse
un exercice papier où l'élève calcule lui-même les attractivités et
probabilités avec des valeurs concrètes. Cet exercice est rare dans
les ressources NSI — il comble le saut cognitif entre « lire une formule »
et « la coder ».

### Les valeurs littérales vérifiées

Toutes les assertions des fonctions de test ont été calculées par force
brute ou algèbre exacte avant d'être écrites dans le TP :

| Assertion | Valeur | Méthode de vérification |
|-----------|--------|------------------------|
| `sac_a_dos(poids, valeurs, 10)` | 20 pts | Force brute sur les 2⁵ = 32 combinaisons |
| `sac_a_dos(poids, valeurs, 2)` | 7 pts | Enumération manuelle |
| Tournée gloutonne A→B→C→D→E→A | 63 km | Calcul étape par étape |
| Tournée optimale A→B→C→E→D→A | 57 km | Force brute sur les 12 permutations |
| `test_ant_system()` | dist ≤ 63 | L'algo doit battre la gloutonne |

---

## Prompt de génération

<details>
<summary><strong>📋 Cliquer pour afficher le prompt complet (2 983 mots)</strong></summary>

> Ce prompt a été construit par rétro-ingénierie à partir de plusieurs cycles
> d'itération et d'audit sur une version initiale. Il produit ce TP en une
> seule génération avec Claude Sonnet ou Opus.

---

```
## RÔLE ET CONTEXTE

Tu es un professeur de NSI (Numérique et Sciences Informatiques) au lycée,
expert en conception de ressources pédagogiques interactives sous forme de
pages HTML autonomes. Tu maîtrises parfaitement le Bulletin Officiel NSI
(arrêtés du 19 juillet 2019 pour la Première et du 17 juillet 2019 pour
la Terminale). Ton objectif est de produire un TP guidé complet et progressif
sur le thème : Les algorithmes de colonies de fourmis — Ant System (Dorigo, 1992).

Thème du programme officiel concerné : Algorithmique — graphes, programmation
dynamique, POO, SQL.
Numéro de ressource : 01

---

## PROFIL DES ÉLÈVES

- Niveau : Terminale NSI
- Pré-requis validés : récursivité Python, listes, dictionnaires, fonctions,
  POO, arbres, graphes, programmation dynamique basique
- Langage : Python uniquement
- Environnement d'exécution : navigateur — PyScript 2026.3.1
- Contrainte critique : une partie des élèves n'a PAS la spécialité
  mathématiques. Toutes les formules doivent être décomposées étape par étape
  avec des exemples numériques concrets. Les symboles Σ, ←, Δ, α, β, η, τ, ρ
  doivent être expliqués en français ordinaire, jamais supposés connus.
- Les élèves n'ont jamais entendu parler du sujet avant ce TP.

---

## OBJECTIFS PÉDAGOGIQUES

À l'issue de ce TP, l'élève sera capable de concevoir et implémenter
un algorithme Ant System : d'abord avec des distances prédéfinies,
puis dans un projet complet avec Pygame.

Capacités BO à couvrir obligatoirement :
- Modéliser un problème à l'aide d'un graphe, appliquer un parcours
- Résoudre un problème d'optimisation par programmation dynamique
- Écrire un programme orienté objet
- Écrire des requêtes SQL : SELECT, INSERT, UPDATE, DELETE

[OBLIGATOIRE] Chaque capacité ci-dessus doit être couverte par au moins
un exercice ou une section de cours.

---

## STRUCTURE DE LA RESSOURCE

Produire exactement ces 9 sections dans cet ordre :

### 1. Introduction — Accroche narrative + Animation JS

- [INTERDIT dans l'intro] : graphe, POO, voyage de commerce, plus court chemin.
  Ces termes n'apparaissent qu'après illustration concrète dans les cours.
- Raconter d'abord le problème du livreur (10 villes → 3 628 800 itinéraires)
  avec des entreprises concrètes (Amazon, Chronopost, ramassage scolaire).
- Raconter l'histoire de Marco Dorigo (1992) observant des fourmis dans son
  jardin, SANS utiliser le mot « graphe ».
- Inclure une animation Canvas JS interactive montrant :
  - 7 villes reliées par des arêtes dont l'épaisseur et la luminosité
    sont proportionnelles aux phéromones (normalisation par le max)
  - Des fourmis visuelles animées comme points colorés se déplaçant
    sur les arêtes entre les itérations
  - Boutons : Démarrer/Pause, Réinitialiser, Accélérer/Ralentir
  - Deux sliders : nombre de fourmis (4–30), taux d'évaporation ρ (0.1–0.9)
  - Compteurs : itération courante, meilleure distance, fourmis actives
  - Une courbe de convergence sur un canvas séparé (distance vs itération)
  - L'animation implémente le VRAI algorithme Ant System (pas une approximation)
- Encadré vocabulaire final (après animation) : phéromone, évaporation,
  heuristique, Ant System, métaheuristique.
- [OBLIGATOIRE] Intégrer en tête de page une image d'illustration
  fournie séparément en base64 (data URI PNG ou JPEG), avec attribut alt
  complet, légende sous l'image, object-fit: cover, object-position: center 30%.

### 2. Cours 1 — Graphes et cycle hamiltonien [BO]

- Graphe complet pondéré, matrice d'adjacence D[i][j]
- [OBLIGATOIRE] Introduire et définir le terme cycle hamiltonien :
  chemin passant par tous les sommets exactement une fois et revenant au départ.
  Formule de longueur avec MathJax : L(T) = Σ D[vk][vk+1]
- Canvas JS statique : dessin du graphe 5 villes avec distances annotées
  + tournée optimale tracée en doré en pointillés
- Encadré : pourquoi la force brute est impossible (12 cycles pour 5 villes,
  60 milliards pour 20 villes, lien avec Held-Karp O(2^n × n²))

### 3. Exercice 1 — Stratégie gloutonne à la main [sans programmation]

- Graphe 5 villes A–E, matrice :
  D = [[0,10,20,15,25],[10,0,8,12,18],[20,8,0,6,10],[15,12,6,0,14],[25,18,10,14,0]]
- [OBLIGATOIRE] La correction doit montrer que :
  - Tournée gloutonne A→B→C→D→E→A = 63 km (valeur vérifiée)
  - Tournée A→B→C→E→D→A = 57 km (OPTIMALE vérifiée par force brute)
  - La gloutonne N'EST PAS l'optimale → leçon pédagogique centrale
- Déblocage : l'élève entre 63 pour déverrouiller. 3 tentatives max.

### 4. Cours 2 — L'algorithme Ant System [BO]

[OBLIGATOIRE — formules pour non-matheux]

Décomposer la formule de probabilité en 5 sous-étapes distinctes,
chacune avec un exemple numérique :

Sous-étape 1 — η (êta) = 1/D[i][j] : si D=10 → η=0.1, si D=20 → η=0.05.

Sous-étape 2 — L'attractivité seule (SANS dénominateur d'abord) :
  Attractivité(i→j) = [τ_ij]^α × [η_ij]^β

Sous-étape 3 — Tableau des paramètres α et β en français :
  α (alpha) | Poids phéromones | Suit la foule (exploitation)
  β (bêta)  | Poids distance   | Privilégie villes proches (exploration)

Sous-étape 4 — Le dénominateur : expliquer Σ en français (additionne tout).

Sous-étape 5 — Formule complète avec MathJax + métaphore du dé truqué.

Évaporation : τ_ij ← (1-ρ) × τ_ij
  Expliquer ← : "remplace la valeur par". Exemple : ρ=0.5, τ=2.0 → 1.0

Dépôt : Δτ_ij^(k) = Q/L_k si arc emprunté, 0 sinon.
  Expliquer Δ : "quantité ajoutée". Logique Q/L_k expliquée.

### 4 bis. Exercice — Calcul de probabilité à la main [OBLIGATOIRE]

Données exactes :
  τ(A→B)=2.0, τ(A→C)=1.0, τ(A→D)=1.5
  D(A→B)=10, D(A→C)=20, D(A→D)=15 ; α=1, β=2

Aide au calcul : donner Attractivité(A→B) = 2.0 × (0.1)² = 0.02

Valeurs de correction vérifiées :
  Attract. A→C = 0.0025, A→D = 0.00667, Somme = 0.02917
  P(A→B) ≈ 68.6 %, P(A→C) ≈ 8.6 %, P(A→D) ≈ 22.9 %

Déblocage : entrer 69 (ou 68). Feedback différencié selon l'erreur.

### 5. Exercice 2 — Sac à dos 0/1 [BO — prog. dynamique]

[OBLIGATOIRE] Encadré doré expliquant "0/1" :
  "Le '0/1' désigne : prendre entièrement (1) ou laisser complètement (0).
  Pas de moitié — tout ou rien." + variante fractionnaire mentionnée.

Données (valeur optimale vérifiée sur 32 combinaisons = 20 pts) :
  noms   = ["Lampe frontale","Gourde","Couteau suisse","Nourriture","Tente"]
  poids  = [2, 3, 1, 5, 6]
  valeurs= [7, 5, 4, 8, 9]
  CAPACITE = 10

Assertions exactes :
  assert v == 0   # capacite=0
  assert v == 7   # capacite=2 → lampe frontale
  assert v == 20  # capacite=10 → optimal

Déblocage à deux paliers :
  Palier 1 : entrer 7 (cap=2) → débloque l'indice de code
  Palier 2 : entrer 20 (cap=10) → débloque la correction complète

### 6. Cours 3 — Mémoïsation vs tabulaire [BO]

Tableau comparatif, deux conditions nécessaires, conseil pratique.
Lien avec les matrices τ et η de l'Ant System.

### 7. Exercice 3 — Implémenter l'Ant System [BO]

Trois parties avec débloqueur distinct :
  Partie A (probabilite_transition) — déblocage : réponse 0.5
  Partie B (construire_tournee) — déblocage : réponse 6
  Partie C (ant_system complet) — déblocage : réponse 0.5
  assert dist <= 63 dans test_ant_system (battre la stratégie gloutonne)

### 8. Synthèse — Projet Pygame + SQL [BO]

[RÈGLE ABSOLUE] Code complet JAMAIS visible au chargement.
Chaque classe : squelette seul (def + docstring + TODO + pass),
correction dans div class="section-masquee" déverrouillée par déblocage.

Débloqueurs :
  Ville    → "Distance entre (0,0) et (3,4) ?" → 5
  Colonie  → "eta[0][2] pour A(0,0),B(3,0),C(3,4) ?" → 0.20
  Visualiseur → "300 iter à 10 fps = ? secondes" → 30
  BaseDonnees → "Mot-clé SQL tri croissant ?" → ORDER BY / ASC

main.py fourni directement (récompense de fin de parcours).
SQL obligatoire : CREATE TABLE, INSERT, SELECT…ORDER BY…LIMIT, DELETE…WHERE.

---

## SYSTÈME DE DÉBLOCAGE — SPÉCIFICATIONS TECHNIQUES

Structure HTML requise pour chaque débloqueur :
  <div class="debloqueur">
    <span class="debloqueur-titre">🔓 Déblocage — [nom]</span>
    <p>[Question]</p>
    <div class="debloqueur-input-row">
      <input class="debloqueur-input" type="[number|text]" id="rep-[id]"
             onkeydown="if(event.key==='Enter')verifier[Id]()">
      <button class="btn-tp-or" onclick="verifier[Id]()">Vérifier</button>
    </div>
    <div class="debloqueur-feedback" id="fb-[id]"></div>
    <div class="tentatives">[N dots]</div>
  </div>
  <div id="correction-[id]" class="section-masquee">[correction]</div>

Comportement obligatoire des verifier() :
  - Feedback différencié selon la valeur (pas juste "incorrect")
  - Dot vert si correct, rouge si raté
  - Toast succès + bordure verte si correct
  - Affichage auto après max tentatives (800ms délai)

---

## CONTRAINTES TECHNIQUES OBLIGATOIRES

CDN exacts :
  Bootstrap 5.3.3 : https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css
  MathJax v3 : CONFIGURATION window.MathJax AVANT le script :
    window.MathJax = {
      tex: { inlineMath:[['\\(','\\)']], displayMath:[['\\[','\\]']] },
      options: { skipHtmlTags:['script','noscript','style','textarea','pre','code'] },
      startup: { typeset:true }
    };
    <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js" async>
  PyScript 2026.3.1 : https://pyscript.net/releases/2026.3.1/core.js

Charte graphique (variables CSS) :
  --bg-main:#1a1f2e  --bg-panel:#242b3d  --bg-cours:#1e2538
  --accent:#8ca5b4   --accent-or:#c9a84c --accent-vert:#4caf50
  --accent-rouge:#e05252  --text-main:#d4dde4  --text-muted:#8a9ab0

Polices : Rubik Dirt (titres) / Source Serif 4 (corps) / JetBrains Mono (code)

UI obligatoire :
  - Navigation sticky (nav.tp-nav) avec lien vers chaque section
  - Bouton retour en haut fixe (#btn-top, visible après 300px scroll)
  - Barre de progression IntersectionObserver (threshold:0.3) sur 9 sections
  - Toggle mode clair/sombre (body.mode-clair)
  - Toast (#toast) bordure verte, disparaît après 2.8s
  - Image entête en base64 : object-fit:cover, object-position:center 30%,
    attribut alt complet, légende sous l'image

Règles HTML :
  - id/class : ASCII pur, sans accent ni espace
  - py-editor : type="py-editor" env="dynamique" exclusivement
  - Styles dans <head>, scripts JS en fin de <body>
  - Fonctionne en file:// (sans serveur)

---

## FONCTIONS DE TEST — RÈGLE ABSOLUE

Tout bloc Python contenant une fonction → fonction test_nom() avec
minimum 3 assertions sur des valeurs CALCULÉES (jamais estimées),
suivie de print("Les tests sont valides."), appelée immédiatement.

Valeurs de référence vérifiées pour CE TP :
  Sac à dos cap=0→0, cap=2→7, cap=10→20 (force brute 32 combos)
  Gloutonne 5 villes → 63 km (calcul étape par étape)
  Optimal 5 villes → 57 km (force brute 12 permutations)
  test_ant_system() → assert dist <= 63

---

## AUDIT INTERNE AVANT GÉNÉRATION

Avant la première ligne HTML, vérifier mentalement :

Mathématique :
  [ ] Toutes assertions calculées, pas estimées
  [ ] Gloutonne = 63 km (étape par étape)
  [ ] Optimal = 57 km (A→B→C→E→D→A, force brute)
  [ ] Sac à dos = 20 pts (32 combinaisons)
  [ ] Probas somment à 100 % (68.6+8.6+22.9)

Pédagogique :
  [ ] Intro sans terme interdit
  [ ] Chaque formule précédée d'explication française
  [ ] "Cycle hamiltonien" défini dans cours 1
  [ ] "0/1" expliqué avec variante fractionnaire
  [ ] Exercice calcul proba existe (section 4 bis)
  [ ] Synthèse : squelettes uniquement, jamais code complet

Technique :
  [ ] window.MathJax configuré avant le script
  [ ] skipHtmlTags inclut 'pre' et 'code'
  [ ] id/class ASCII pur
  [ ] Image base64 avec alt complet
  [ ] IntersectionObserver sur 9 sections

---

## FORMAT DE SORTIE

Page HTML complète (<!DOCTYPE html> à </html>), autonome, sans serveur.
Pied de page exact : « Lycée Antoine Watteau — DarkSATHI Li »

Ton : pédagogique, bienveillant, tutoiement, questions rhétoriques.
Un concept nouveau maximum par paragraphe.
Chaque section commence par une phrase de liaison avec la précédente.
Typographie française : accents, guillemets « », apostrophe courbe '.
```

</details>

---

## Ce que ce prompt apporte par rapport à un prompt classique

Ce prompt a été construit par **rétro-ingénierie** à partir de plusieurs cycles
d'itération et d'audit sur une version initiale. Il contient 26 contraintes
absentes de la version initiale, découvertes une par une lors des échanges.

Les trois catégories de contraintes les plus importantes :

### 1. Vérification mathématique obligatoire

Le prompt impose de **calculer toutes les valeurs par force brute** avant de
les écrire dans une assertion. Sans cette contrainte, un modèle peut écrire
`assert v == 20` pour un sac à dos dont l'optimal réel est 16 — les tests
plantent pour un code correct.

### 2. Règles négatives sur les corrections

Un prompt classique décrit ce qu'on produit. Ce prompt décrit aussi ce
qu'on **interdit** : le code complet de la synthèse n'est jamais visible,
les corrections sont verrouillées derrière une réponse. Ces règles négatives
sont souvent plus importantes que les règles positives.

### 3. Traduction des formules en contraintes concrètes

La contrainte "élèves sans maths" était dans la version initiale, mais
non traduite en exigences vérifiables. Ce prompt la traduit en actions
précises : 5 sous-étapes pour décomposer la formule de probabilité,
définition de Σ en français, exercice de calcul intermédiaire obligatoire.

---

## Licence

Ressource pédagogique libre — utilisation, modification et redistribution
autorisées pour tout usage éducatif non commercial.

---

*Lycée Antoine Watteau, Valenciennes — DarkSATHI Li*  
*Généré avec Claude Sonnet · Prompt engineering documenté*
