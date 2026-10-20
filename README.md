# Novinzo starter theme

The starter theme for [Novinzo](https://novinzo.com) storefronts: Liquid templates styled
with Tailwind CSS, deployed to your store by GitHub Actions on every push to `main`.

## Set up

1. Click **Use this template → Create a new repository** to make your own copy.
2. In the Novinzo dashboard, open **Settings → Theme** and link the new repository and its
   `main` branch.
3. In the repository, add two variables under **Settings → Secrets and variables →
   Actions → Variables**: `NOVINZO_STORE` and `NOVINZO_API_URL`. The dashboard's Theme page
   shows their values. No secret is needed: the deploy proves which repository it runs for
   with GitHub's OIDC token.
4. Push to `main`. The first run deploys nothing yet: approve it on the dashboard's
   Theme page, which shows the repository, who started the run and its commit, so a
   repository that only shares this one's name cannot deploy to your store.
5. Re-run the workflow. From now on every push builds the theme, checks it and deploys it.
   Problems show as annotations on the lines they are about.

## Work on the theme

You need Node.js 22 or later. Run `npm install` once.

- `layouts/`, `templates/`, `sections/` and `snippets/` are the theme's Liquid. Every page
  the storefront renders has a template; the deploy refuses a theme missing one.
- `assets/input.css` is the Tailwind source; `npm run build` compiles it into
  `assets/theme.css`, which the layout loads. `npm run watch` rebuilds as you edit.
- Other files in `assets/` (images, fonts, scripts) are served from your store at
  `/theme/assets/…`; use the `asset_url` filter to link them.
- `theme.json` names the theme.

To check the theme without deploying, clone
[`emergente-labs/novinzo-theme-deploy`](https://github.com/emergente-labs/novinzo-theme-deploy)
next to this repository and run `npm run check` here. Pull requests run the same check.

## Roll back

Revert the commit and push. Novinzo keeps only the live version of your theme.

## Updates to the starter

To take in later improvements, add this repository as a remote and merge its tagged
releases. Novinzo announces breaking changes to the variables templates can read ahead of
time, and flags themes that use them on the dashboard's Theme page.

## License

The theme is [MIT licensed](LICENSE). The Inter and JetBrains Mono fonts in `assets/` are
under the [SIL Open Font License](OFL.txt).
