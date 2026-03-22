# PROMPT EXEMPLAIRE — TP NSI Terminale : Algorithme de Colonies de Fourmis

> **Usage :** Coller l'intégralité de ce prompt dans Claude (Sonnet ou Opus).
> Il produit en une seule génération le TP complet, sans itération corrective.
> Durée de génération estimée : 3–5 minutes selon le modèle.

---

## RÔLE ET CONTEXTE

Tu es un professeur de NSI (Numérique et Sciences Informatiques) au lycée,
expert en conception de ressources pédagogiques interactives sous forme de
pages HTML autonomes. Tu maîtrises parfaitement le Bulletin Officiel NSI
(arrêtés du 19 juillet 2019 pour la Première et du 17 juillet 2019 pour
la Terminale). Ton objectif est de produire un TP guidé complet et progressif
sur le thème : **Les algorithmes de colonies de fourmis — Ant System (Dorigo, 1992)**.

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
- **Contrainte critique :** une partie des élèves n'a PAS la spécialité
  mathématiques. Toutes les formules doivent être décomposées étape par étape
  avec des exemples numériques concrets. Les symboles Σ, ←, Δ, α, β, η, τ, ρ
  doivent être expliqués en français ordinaire, jamais supposés connus.
- Les élèves n'ont jamais entendu parler du sujet avant ce TP.

---

## OBJECTIFS PÉDAGOGIQUES

À l'issue de ce TP, l'élève sera capable de concevoir et implémenter
un algorithme Ant System : d'abord avec des distances prédéfinies,
puis dans un projet complet avec Pygame.

### Capacités BO à couvrir obligatoirement

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
- [OBLIGATOIRE] Introduire et définir le terme **cycle hamiltonien** :
  chemin passant par tous les sommets exactement une fois et revenant au départ.
  Formule de longueur avec MathJax : L(T) = Σ D[vk][vk+1]
- Canvas JS statique : dessin du graphe 5 villes avec distances annotées
  + tournée optimale tracée en doré en pointillés
- Encadré : pourquoi la force brute est impossible (12 cycles pour 5 villes,
  60 milliards pour 20 villes, lien avec Held-Karp O(2^n × n²))

### 3. Exercice 1 — Stratégie gloutonne à la main [sans programmation]

- Graphe 5 villes A–E avec cette matrice exacte (distances en km) :
  D = [[0,10,20,15,25],[10,0,8,12,18],[20,8,0,6,10],[15,12,6,0,14],[25,18,10,14,0]]
- Questions guidées : appliquer la stratégie gloutonne depuis A,
  calculer la distance totale, comparer avec A→B→C→E→D→A.
- [OBLIGATOIRE] La correction doit montrer que :
  - Tournée gloutonne A→B→C→D→E→A = **63 km** (valeur correcte vérifiée)
  - Tournée A→B→C→E→D→A = **57 km** (tournée OPTIMALE vérifiée par force brute)
  - La gloutonne N'EST PAS l'optimale → c'est la leçon pédagogique centrale
- Système de déblocage : l'élève entre 63 (distance gloutonne) pour déverrouiller
  la correction. 3 tentatives max, puis affichage automatique.

### 4. Cours 2 — L'algorithme Ant System [BO]

[OBLIGATOIRE — formules pour non-matheux]

Décomposer la formule de probabilité en 5 sous-étapes distinctes,
chacune avec un exemple numérique :

**Sous-étape 1** — η (êta) = 1/D[i][j] : montrer que si D=10 → η=0.1
et D=20 → η=0.05. La ville la plus proche a le plus grand η.

**Sous-étape 2** — L'attractivité seule :
  Attractivité(i→j) = [τ_ij]^α × [η_ij]^β
  Présenter d'abord cette formule SANS le dénominateur.

**Sous-étape 3** — Tableau des paramètres α et β :
  | Paramètre | Rôle | Effet si élevé |
  α (alpha) | Poids phéromones | Suit la foule (exploitation)
  β (bêta)  | Poids distance   | Privilégie villes proches (exploration)
  Expliquer en français : "α élevé → les fourmis font confiance à l'historique"

**Sous-étape 4** — Le dénominateur = SOMME des attractivités disponibles.
  Expliquer le symbole Σ en français : "additionne pour chaque ville
  non encore visitée".

**Sous-étape 5** — La formule complète avec MathJax :
  P(i→j) = [τ_ij]^α × [η_ij]^β / Σ_{k non visité} [τ_ik]^α × [η_ik]^β
  Métaphore du dé truqué : faces inégales, probabilité proportionnelle au poids.

Évaporation : τ_ij ← (1-ρ) × τ_ij
  Expliquer la flèche ← : "remplace la valeur par". Exemple numérique :
  ρ=0.5, τ=2.0 → nouveau τ = (1-0.5)×2.0 = 1.0

Dépôt : Δτ_ij^(k) = Q/L_k si la fourmi k a emprunté l'arc, 0 sinon.
  Expliquer Δ (delta) : "quantité ajoutée". Expliquer pourquoi Q/L_k :
  "tournée courte → L_k petit → Q/L_k grand → beaucoup de phéromones".

Lien avec prog. dynamique : Held-Karp exact mais O(2^n·n²) → impraticable.

### 4 bis. Exercice de calcul de probabilité à la main [NOUVEAU — obligatoire]

[OBLIGATOIRE] Cet exercice est absent de la plupart des TPs NSI
et constitue une valeur ajoutée pédagogique majeure.

- Situation : fourmi en A, peut aller en B, C ou D (E déjà visitée).
- Données exactes à utiliser :
  τ(A→B)=2.0, τ(A→C)=1.0, τ(A→D)=1.5
  D(A→B)=10, D(A→C)=20, D(A→D)=15 → η=0.1, 0.05, 1/15≈0.0667
  α=1, β=2
- Aide au calcul : donner le premier terme calculé :
  Attractivité(A→B) = 2.0 × (0.1)² = 2.0 × 0.01 = **0.02**
  L'élève calcule A→C et A→D de la même façon.
- Valeurs exactes de la correction (vérifiées) :
  Attractivité A→C = 1.0 × (0.05)² = 0.0025
  Attractivité A→D = 1.5 × (1/15)² = 0.00667
  Dénominateur = 0.02 + 0.0025 + 0.00667 = 0.02917
  P(A→B) ≈ **68.6 %**, P(A→C) ≈ 8.6 %, P(A→D) ≈ 22.9 %
- Explication finale : β=2 élève au carré → B 4× plus attractif que C sur
  le critère distance, même si phéromones seulement 2× plus élevées.
- Déblocage : l'élève entre 69 (ou 68) pour déverrouiller la correction.
  3 tentatives, feedback différencié :
  - Si val < 50 : "B a plus de phéromones ET est plus proche, sa proba
    dépasse 50 %"
  - Si val > 80 : "Un peu trop élevé, recalcule l'attractivité de D"
  - Sinon : "Recalcule le dénominateur"

### 5. Exercice 2 — Sac à dos 0/1 [BO — prog. dynamique]

[OBLIGATOIRE] Expliquer "0/1" dès l'introduction de l'exercice :
  Un encadré doré doit définir : "Le '0/1' désigne les deux seuls choix
  pour chaque objet : soit on le prend entièrement (1), soit on le laisse
  complètement (0). Pas de moitié — c'est tout ou rien."
  Mentionner la variante fractionnaire et pourquoi elle est plus facile
  (stratégie gloutonne suffit).

- Transition narrative depuis le cours 2 : expliquer pourquoi cet exercice
  arrive ici (entraîner la prog. dynamique avant de coder l'Ant System).

- Données exactes à utiliser (valeur optimale vérifiée = 20 pts) :
  noms   = ["Lampe frontale", "Gourde", "Couteau suisse", "Nourriture", "Tente"]
  poids  = [2, 3, 1, 5, 6]
  valeurs= [7, 5, 4, 8, 9]
  CAPACITE = 10
  Optimal : Lampe(2,7) + Gourde(3,5) + Nourriture(5,8) = 10 kg, 20 pts

- Assertions EXACTES à utiliser (vérifiées par calcul) :
  assert v == 0  (pour capacite=0)
  assert v == 7  (pour capacite=2 → lampe frontale seulement)
  assert v == 20 (pour capacite=10 → optimal)

- Déblocage à deux paliers :
  Palier 1 : entre 7 (cap=2) → débloque l'indice de code
  Palier 2 : entre 20 (cap=10) → débloque la correction complète

### 6. Cours 3 — Mémoïsation vs tabulaire [BO]

- Tableau comparatif des deux approches
- Les deux conditions nécessaires à la prog. dynamique
- Conseil pratique : quand utiliser chaque approche
- Lien avec les matrices τ et η utilisées dans l'Ant System

### 7. Exercice 3 — Implémenter l'Ant System [BO]

Trois parties indépendantes, chacune avec son débloqueur :

**Partie A — probabilite_transition()**
  Question déblocage : "Avec tau et eta uniformes à 1, alpha=1, beta=1,
  depuis ville 0 vers [1,2] : quelle est la probabilité d'aller en 1 ?"
  Réponse : 0.5

**Partie B — construire_tournee()**
  Question déblocage : "Combien d'éléments dans la liste tournee
  pour un graphe à 5 villes ?" Réponse : 6

**Partie C — mettre_a_jour_pheromones() + ant_system()**
  Question déblocage : "Quelle est la valeur de tau[i][j] après une
  évaporation avec ρ=0.5, en partant de tau=1.0 ?" Réponse : 0.5

Assertion clé de test_ant_system() :
  assert dist <= 63  (doit faire mieux que la stratégie gloutonne)
  assert dist_calculee == dist  (cohérence de la distance retournée)

### 8. Synthèse — Projet Pygame + SQL [BO]

[OBLIGATOIRE — RÈGLE ABSOLUE] :
Le code complet des classes Ville, Colonie, Visualiseur et BaseDonnees
NE DOIT PAS être visible au chargement de la page.
Chaque classe est présentée sous forme de SQUELETTE UNIQUEMENT
(def + docstring + pass + TODO) suivi de son propre débloqueur.
Le code correct n'apparaît que dans une div class="section-masquee"
déverrouillée par le débloqueur.

Débloqueurs de la synthèse (questions simples, réponses numériques) :
  Classe Ville → "Quelle est la distance entre (0,0) et (3,4) ?" → 5
  Classe Colonie → "Quelle est la valeur de eta[0][2] pour les villes
    A(0,0), B(3,0), C(3,4) ?" → 0.20
  Visualiseur → "300 itérations à 10 fps = combien de secondes ?" → 30
  BaseDonnees → "Quel mot-clé SQL trie du plus petit au plus grand ?" → ORDER BY / ASC

Le seul code donné directement (sans déblocage) : main.py.
C'est la récompense de fin de parcours — il ne fonctionne que si les
4 classes ont été correctement implémentées.

SQL couvrir obligatoirement : CREATE TABLE, INSERT, SELECT...ORDER BY...LIMIT,
DELETE...WHERE.

---

## SYSTÈME DE DÉBLOCAGE — SPÉCIFICATIONS TECHNIQUES

[OBLIGATOIRE] Chaque exercice et chaque étape de la synthèse dispose
d'un système de déblocage avec les propriétés suivantes :

```
Structure HTML requise :
<div class="debloqueur">
  <span class="debloqueur-titre">🔓 Déblocage — [nom]</span>
  <p>[Question à résoudre]</p>
  <div class="debloqueur-input-row">
    <input class="debloqueur-input" type="[number|text]" id="rep-[id]"
           onkeydown="if(event.key==='Enter')verifier[Id]()">
    <button class="btn-tp-or" onclick="verifier[Id]()">Vérifier</button>
  </div>
  <div class="debloqueur-feedback" id="fb-[id]"></div>
  <div class="tentatives">
    [N dots] tentatives avant affichage automatique
  </div>
</div>
<div id="correction-[id]" class="section-masquee">
  [correction complète]
</div>
```

Comportement obligatoire des fonctions verifier() :
- Comparer la valeur entrée à la valeur attendue (avec tolérance ±1 pour les %)
- Marquer le dot correspondant en vert (ok) ou rouge (used)
- Si correct : afficher un toast "✓ [nom] débloqué !", changer la bordure
  du débloqueur en vert, afficher la div correction
- Si incorrect et tentatives < max : afficher un feedback différencié selon
  l'erreur (pas juste "incorrect")
- Si tentatives = max : afficher la correction automatiquement après 800ms,
  bordure rouge

Toast requis : notification en bas à droite, fond panel, bordure verte,
disparaît après 2.8 secondes.
```

---

## CONTRAINTES TECHNIQUES OBLIGATOIRES

### CDN et versions exactes

```html
Bootstrap 5.3.3 →
  https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css
Google Fonts → Rubik Dirt + Source Serif 4 + JetBrains Mono
MathJax v3 → configuration OBLIGATOIRE avant le script :
  <script>
    window.MathJax = {
      tex: { inlineMath: [['\\(','\\)']], displayMath: [['\\[','\\]']] },
      options: { skipHtmlTags: ['script','noscript','style','textarea','pre','code'] },
      startup: { typeset: true }
    };
  </script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"
          id="MathJax-script" async></script>
PyScript 2026.3.1 → https://pyscript.net/releases/2026.3.1/core.js
```

### Charte graphique

```css
--bg-main:     #1a1f2e
--bg-panel:    #242b3d
--bg-cours:    #1e2538
--accent:      #8ca5b4
--accent-or:   #c9a84c
--accent-vert: #4caf50
--accent-rouge:#e05252
--text-main:   #d4dde4
--text-muted:  #8a9ab0
```

Titres h1–h4 : Rubik Dirt
Corps de texte : Source Serif 4
Code, boutons, navigation : JetBrains Mono

### Composants UI requis

```
.encadre        → cours (bordure --accent)
.encadre-conseil → remarque (bordure --accent-or)
.encadre-indice → indice (bordure --accent-vert)
.encadre-danger → avertissement (bordure --accent-rouge)
Chaque encadré : .encadre-prefix en petites capitales (font-variant: small-caps)
Boutons : .btn-tp / .btn-tp-or / .btn-tp-vert / .btn-tp-rouge / .btn-secondaire
```

### Éléments d'interface obligatoires

- Navigation sticky avec liens vers chaque section (nav.tp-nav)
- Bouton retour en haut (#btn-top) fixe en bas à droite, visible après 300px scroll
- Barre de progression (#barre-progression) via IntersectionObserver,
  mise à jour à chaque section visible (threshold: 0.3)
- Compteur "X / 9 sections" à côté de la barre
- Bouton toggle mode clair/sombre (#btn-mode)
- Toast (#toast) pour les confirmations de déblocage
- Mode clair en body.mode-clair avec variables CSS remappées

### Image d'entête

```html
<img
  src="[DATA_URI base64 fournie]"
  alt="[description accessible complète]"
  class="tp-header-img"
  loading="eager"
>
<p class="tp-header-img-legende">« Au cœur du système ! » — L'Ant System vu de l'intérieur</p>

/* CSS : */
.tp-header-img {
  width: 100%;
  max-height: 320px;
  object-fit: cover;
  object-position: center 30%;
  border-radius: var(--radius);
  border: 1px solid var(--accent);
}
```

### Règles HTML critiques

- id et class : ASCII strict, sans accent, sans espace, sans caractère spécial
- Fonctions JavaScript : camelCase ASCII uniquement
- Page fonctionnelle en mode fichier local (file://)
- Styles dans `<style>` dans le `<head>`, scripts JS en fin de `<body>`
- Balise éditeur Python : `<script type="py-editor" env="dynamique">` exclusivement
- MathJax : formules display dans `<div class="formule-bloc">\[ ... \]</div>`
  et formules inline \( ... \) dans le texte courant

---

## CONTRAINTES DES EXERCICES

### Structure obligatoire de chaque exercice

- Mise en situation narrative (italique, bordure or)
- Énoncé numéroté
- Système de déblocage (voir spécifications ci-dessus)
- Éditeur PyScript exécutable
- Correction cachée déverrouillable par déblocage

### Fonctions de test — RÈGLE ABSOLUE

[OBLIGATOIRE] Tout bloc Python contenant une fonction doit se terminer
par une fonction `test_nom()` avec minimum 3 assertions sur des valeurs
littérales **pré-calculées et vérifiées par calcul exact** (jamais estimées),
suivie de `print("Les tests sont valides.")`, appelée immédiatement.

[OBLIGATOIRE] Avant d'écrire toute valeur dans une assertion :
1. Calculer la valeur par force brute ou algèbre exacte
2. Vérifier que cette valeur est cohérente avec l'algorithme implémenté
3. Ne jamais écrire une valeur "de mémoire" ou "intuitivement"

Exemples de valeurs vérifiées dans CE TP :
- Sac à dos cap=0 → 0, cap=2 → 7, cap=10 → **20** (force brute sur 32 combos)
- Tournée gloutonne 5 villes → **63 km** (calcul étape par étape)
- Tournée optimale 5 villes → **57 km** (force brute sur 12 permutations)
- Tournée gagne-t-elle sur 63 km ? → assert dist <= 63 dans test_ant_system

---

## EXIGENCES LINGUISTIQUES

### Typographie

- Français correct, accents obligatoires : é, è, ê, à, â, î, ô, û, ù, ç, œ
- Guillemets français : « »
- Apostrophe courbe : '
- Tiret demi-cadratin : –
- Aucune faute d'accord

### Ton et registre

- Pédagogique, bienveillant, direct — comme un prof qui parle à l'oral
- Tutoiement (« tu »)
- Questions rhétoriques pour maintenir l'attention
- L'élève ressent le problème AVANT de recevoir la solution
- Maximum un concept nouveau par paragraphe
- Chaque section commence par rappeler où on en est (phrase de liaison)

---

## AUDIT INTERNE OBLIGATOIRE AVANT GÉNÉRATION

[OBLIGATOIRE] Avant d'écrire la première ligne de HTML, effectuer
mentalement l'audit suivant. Si un point échoue, corriger avant de continuer.

### Vérification mathématique
- [ ] Toutes les valeurs des assertions ont été calculées, pas estimées
- [ ] La tournée gloutonne calculée étape par étape depuis A = 63 km
- [ ] La tournée optimale vérifiée par force brute = 57 km (A→B→C→E→D→A)
- [ ] L'optimal sac à dos vérifié sur toutes les 2^5=32 combinaisons = 20 pts
- [ ] Les trois probabilités du calc. proba somment à 100 % : 68.6+8.6+22.9≈100

### Vérification pédagogique
- [ ] L'introduction ne contient aucun terme technique interdit
- [ ] Chaque formule est précédée d'une explication en français
- [ ] Le terme "cycle hamiltonien" est défini dans le cours 1
- [ ] Le terme "0/1" est expliqué avec la variante fractionnaire
- [ ] L'exercice de calcul de proba existe entre le cours 2 et l'exercice 2
- [ ] La synthèse ne montre que des squelettes, jamais le code complet

### Vérification technique
- [ ] window.MathJax configuré avant le script mathjax
- [ ] skipHtmlTags inclut 'pre' et 'code'
- [ ] Tous les id/class sont en ASCII pur
- [ ] L'image est intégrée en base64 avec attribut alt complet
- [ ] La barre de progression observe les 9 sections correctes

---

## FORMAT DE SORTIE

Produire une seule page HTML complète et autonome (de `<!DOCTYPE html>` à `</html>`).
- Fonctionne sans serveur, directement dans un navigateur moderne
- Styles dans `<style>` dans le `<head>`, scripts JS en fin de `<body>`
- Pied de page exactement : « Lycée Antoine Watteau — DarkSATHI Li »

---

## NOTE FINALE AU MODÈLE

Ce prompt est le résultat de plusieurs cycles d'itération et d'audit
sur une version initiale moins précise. Chaque contrainte ici est le
fruit d'une erreur détectée, d'une remarque pédagogique ou d'une
amélioration validée. Respecte-les toutes sans exception — elles ne
sont pas redondantes, elles sont complémentaires.

La qualité visée est celle d'une ressource qu'un professeur pourrait
distribuer telle quelle en classe le lendemain, sans correction.
