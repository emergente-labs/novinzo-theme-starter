# Novinzo theme

This repository is your Novinzo store's theme. Every push to `main` deploys it to your
store through GitHub Actions.

## Set up

1. In the Novinzo dashboard, open **Settings → Theme** and link this repository and its
   `main` branch.
2. In this repository, add two variables under **Settings → Secrets and variables →
   Actions → Variables**: `NOVINZO_STORE` and `NOVINZO_API_URL`. The dashboard's Theme page
   shows their values.
3. Push to `main`. The first run deploys nothing yet: approve it on the dashboard's
   Theme page, which shows the repository, who started the run and its commit, so a
   repository that only shares this one's name cannot deploy to your store.
4. Re-run the workflow. From now on every push builds the theme, checks it and deploys it.
   Problems show as annotations on the lines they are about.

## Work on the theme

- `layouts/`, `templates/`, `sections/` and `snippets/` are the theme's Liquid. Every page
  the storefront renders has a template; the deploy refuses a theme missing one.
- `assets/input.css` is the Tailwind source; `npm run build` compiles it into
  `assets/theme.css`, which the layout loads. `npm run watch` rebuilds as you edit.
- Other files in `assets/` (images, fonts, scripts) are served from your store at
  `/theme/assets/…`; use the `asset_url` filter to link them.
- `theme.json` names the theme.

To check the theme without deploying, clone `emergente-labs/novinzo-theme-deploy` next to
this repository and run `node ../novinzo-theme-deploy/dist/index.mjs check .` here.

## Roll back

Revert the commit and push. Novinzo keeps only the live version of your theme.

## Updates to the starter

This theme started from `emergente-labs/novinzo-theme-starter`. To take in later
improvements, add it as a remote and merge its tagged releases. Novinzo announces
breaking changes to the variables templates can read ahead of time, and flags themes that
use them on the dashboard's Theme page.

## License

The theme is [MIT licensed](LICENSE). The Inter and JetBrains Mono fonts in `assets/` are
under the [SIL Open Font License](OFL.txt).
