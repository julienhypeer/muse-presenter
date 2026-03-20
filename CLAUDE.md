# Presenter

Outil de présentation pour vidéos YouTube. Affiché à l'écran pendant le tournage (côté droit = visible à la caméra).

## Architecture

- **Single-page app** : `index.html` (HTML + CSS + JS vanilla) + `data.json` (contenu)
- Servi sur http://69.62.109.87:8090 via un serveur statique
- Pas de framework, pas de build step

## Système de rendu des slides

### Canvas fixe 960x540 avec auto-scaling

**C'est la règle la plus importante du projet.** Les slides sont designées dans un canvas de taille fixe (960x540px, ratio 16:9). Un `transform: scale()` JS adapte le canvas à l'espace disponible. Cela garantit :

- **Zéro overflow** — le contenu ne déborde jamais
- **Proportions identiques** sur n'importe quel écran (MacBook demi-écran, moniteur externe, projecteur)
- **Tailles de police en pixels fixes** — prévisibles, pas de `rem` ni `vw` dans le slide

**Ne jamais utiliser de tailles relatives (`rem`, `em`, `vw`, `%`) pour le contenu des slides.** Toujours des `px`.

### Syntaxes enrichies dans le contenu

- `{{bg:N}}` — chiffre/symbole décoratif géant en fond (200px, opacity 6%)
- `{{keyword:MOT}}` — texte géant en accent (56px, weight 900)
- `{{stat:VALEUR|label}}` — chiffre principal + sous-label
- `{{hl:texte}}` — highlight inline (fond orange, texte noir)
- Markdown standard : `# H1`, `## H2`, `> blockquote`, `---`, `**bold**`, `*italic*`, `- liste`

### Layouts par slide

Champ `layout` dans data.json :
- `"default"` — rendu standard
- `"statement"` — texte centré, plus grand
- `"quote"` — citation plein écran avec guillemet décoratif
- `"stats"` — chiffres en grand, centré
- `"list"` — bullets accentués

## Interface

- **Moitié gauche** : prompteur (notes de tournage) + contrôles (admin, navigation slides, navigation notes)
- **Moitié droite** : slide visible à la caméra — **rien d'autre** (pas de boutons, pas de toolbar)
- La barre de progression (3px en haut) et les éléments de slide sont les seuls éléments visibles côté droit
- Navigation clavier : flèches gauche/droite pour slides, haut/bas pour notes, `A` pour admin

## Conventions

- **Sentence case** pour les titres de vidéo et de slides (ex: "Les 5 piliers du closing", pas "Les 5 Piliers Du Closing")
- **Accents français corrects** partout (é, è, ê, à, ù, ç, etc.)
- Première slide de chaque vidéo = titre de la vidéo (layout `statement`)
- Le nom de l'outil est "Presenter"
