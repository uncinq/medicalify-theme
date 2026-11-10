# medicalify-theme

## Install

Edit `config/_default/module.yaml` to install the `medicalify-theme` module.

### V2

This module carries both styling layers, so it works with either design module. Declare it **first**, before the section modules, the core theme and the styling module: it overrides partials of the first two, and its mounts have to shadow theirs.

```yml
imports:
  - path: github.com/uncinq/medicalify-theme/v2

  # Section modules
  - path: github.com/hugolify/hugolify-theme-persons/v2

  # Core theme
  - path: github.com/hugolify/hugolify-theme/v2

  # Styling, one of the two
  - path: github.com/hugolify/hugolify-theme-design-system # CSS + design tokens
  # - path: github.com/hugolify/hugolify-theme-bootstrap   # SASS + Bootstrap 5
```

With **hugolify-theme-design-system** the theme comes from `assets/css/` and `assets/tokens/`; with **hugolify-theme-bootstrap** it comes from `assets/sass/`.

### V1

```yml
imports:
  - path: github.com/uncinq/medicalify-theme
  - path: github.com/hugolify/hugolify-theme
```

## Upgrading a site from v1

The diplomas of a person moved from the theme's own `diplomas` field, a block of markdown, to hugolify-admin's `education` field, a list the core theme also publishes as structured data (`hasCredential`, `alumniOf`). hugolify-theme-persons lists them at the top of the sidebar of the person page, under its own heading ("Formations" in French). Convert each person's front matter by hand:

```yml
# v1
diplomas: >-
  * 2010 - D.E.S.C. Chirurgie orthopédique – Université Bordeaux Segalen

# v2
education:
  - title: D.E.S.C. Chirurgie orthopédique
    school: Université Bordeaux Segalen
    year: 2010
    category: degree # or certificate
```

Each entry is one diploma: a position held or the summary of a thesis belongs in the body of the page. A `diplomas` key left in place is no longer shown, nor editable in the CMS.

## Design tokens

The design-system layer is authored as [DTCG](https://www.designtokens.org/) token files under `assets/tokens/theme/` and compiled to custom properties with [Style Dictionary](https://styledictionary.com/):

```bash
npm install
npm run build
```

This writes `assets/css/tokens/theme/**.css` plus the `assets/css/tokens/theme.css` barrel, both committed: Hugo reads the generated CSS, never the JSON. Run it after touching any token file, and never edit the generated CSS by hand: the next build overwrites it.

These tools only build the tokens here, a site never needs them: `hugo mod npm pack` reads `package.hugo.json`, which lists no dependency, instead of `package.json`.

Hand-written CSS lives in `assets/css/theme/` and is imported by `assets/css/theme.css`. Values that are design decisions belong in a token file, not in the CSS; each token names the SASS variable it replaces.

## Releasing

Tags are `v`-prefixed (`v2.0.0`): a `/v2` module path is only resolvable from a `v2.x.y` semver tag. The v1 tags (`1.0.7`) have none.

```bash
npx release-it
```

## Documentation

- [Hugolify documentation](https://www.hugolify.io/docs/)
- [Migration from v1 to v2](https://www.hugolify.io/docs/getting-started/migration/)

## Licensing

Hugolify is free for personal or commercial projects (MIT license)
