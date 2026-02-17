# Demo SCSS

Mini projet de démonstration HTML + SCSS avec:
- architecture SCSS simple et modulaire
- conventions de nommage BEM
- design tokens centralisés dans `vars/_vars.scss`

## Prérequis

- Node.js 18+ (recommandé)
- PNPM (le projet utilise `pnpm@10.28.0`)

## Workflow PNPM

Installation:

```bash
pnpm install
```

Lancer la compilation SCSS en mode watch:

```bash
pnpm run sass
```

Ce script compile:
- entrée: `assets/scss/style.scss`
- sortie: `assets/css/style.css`
- options: `--watch --no-source-map --style=compressed`

Compilation ponctuelle (sans watch):

```bash
pnpm exec sass --no-source-map --style=compressed assets/scss/style.scss assets/css/style.css
```

## Workflow GitHub Actions

Le projet inclut un workflow CI:

- fichier: `.github/workflows/build-css.yml`
- déclenchement:
  - `push` sur `main`
  - `pull_request`
- actions exécutées:
  - installation PNPM + Node
  - `pnpm install --frozen-lockfile`
  - compilation CSS sans source map

Commande utilisée en CI:

```bash
pnpm exec sass --no-source-map --style=compressed assets/scss/style.scss assets/css/style.css
```

## Architecture SCSS

```text
assets/
├─ scss/
│  ├─ style.scss                # Point d'entrée SCSS
│  ├─ vars/
│  │  └─ _vars.scss             # Design tokens (couleurs, spacing, tailles, breakpoints...)
│  ├─ layout/
│  │  ├─ _header.scss           # Layout global header
│  │  └─ _footer.scss           # Layout global footer
│  └─ components/
│     ├─ _hero-grid.scss        # Composant hero
│     └─ _stack.scss            # Composant stack + card
└─ css/
   └─ style.css                 # Fichier compilé (généré)
```

## Stratégie BEM utilisée

Convention:
- `block`
- `block__element`
- `block--modifier`

Exemples du projet:
- `site-header`, `site-header__nav`, `site-header__list`, `site-header__link`
- `hero-grid`, `hero-grid__title`, `hero-grid__content`, `hero-grid__picture`
- `stack`, `stack__section`, `stack__section--first`, `stack__section--second`, `stack__section--third`
- `site-footer`, `site-footer__column`

Règles appliquées:
- un bloc décrit une zone fonctionnelle autonome
- les éléments (`__`) restent liés à leur bloc
- les modificateurs (`--`) décrivent une variante d’état/style
- éviter les styles basés uniquement sur les balises HTML (`header`, `footer`, `section`, etc.)

## Design system (variables SCSS)

Tous les tokens UI sont centralisés dans:

`assets/scss/vars/_vars.scss`

Catégories actuelles:
- typographie (`$font-family-base`)
- couleurs (`$color-*`)
- espacements (`$space-*`)
- dimensions et layout (`$radius-lg`, `$footer-height`, `$hero-max-width`, `$breakpoint-sm`, etc.)

## Structure de l'entrée SCSS

`assets/scss/style.scss` importe les modules dans cet ordre:
1. `vars/vars`
2. `layout/header`
3. `components/hero-grid`
4. `components/stack`
5. `layout/footer`

Puis définit les styles globaux de page (`.page`).

## Bonnes pratiques pour continuer

- ajouter un composant = créer un partial dans `components/` puis l’importer dans `style.scss`
- ajouter un layout global = créer dans `layout/`
- toute valeur UI réutilisable doit d’abord être un token dans `vars/_vars.scss`
- garder la convention BEM systématique pour limiter les conflits CSS
