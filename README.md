# Novinzo starter theme

The starter theme for [Novinzo](https://novinzo.com) storefronts: Liquid templates styled
with Tailwind CSS, deployed to your store by GitHub Actions on every push to `main`.

## Set up

1. Click **Use this template → Create a new repository** to make your own copy.
2. In the Novinzo dashboard, open **Settings → Theme** and link the new repository and its
   `main` branch.
3. In the repository, add the variable `NOVINZO_STORE` under **Settings → Secrets and
   variables → Actions → Variables**, set to your store's subdomain as the dashboard's Theme
   page shows it. No secret is needed: the deploy proves which repository it runs for with
   GitHub's OIDC token.
4. Push to `main`. The first run deploys nothing yet: approve it on the dashboard's
   Theme page, which shows the repository, who started the run and its commit, so a
   repository that only shares this one's name cannot deploy to your store.
5. Re-run the workflow. From now on every push builds the theme, checks it and deploys it.
   Problems show as annotations on the lines they are about.

## Work on the theme

You need Node.js 22 or later. Run `npm install` once.

- `layouts/`, `templates/`, `sections/` and `snippets/` are the theme's Liquid.
- `assets/input.css` is the Tailwind source; `npm run build` compiles it into
  `assets/theme.css`, which the layout loads. `npm run watch` rebuilds as you edit.
- Other files in `assets/` (images, fonts, scripts) are served from your store at
  `/theme/assets/…`; use the `asset_url` filter to link them.
- `theme.json` names the theme and the starter release it `extends` (see below).

To preview the theme on your store as you edit it, without deploying, run
`npx @novinzo/theme-cli dev`. `npm run check` has Novinzo check the theme the way a deploy
does, and changes nothing on your store. Pull requests run the same check, as
`.github/workflows/check.yml`.

## Roll back

Revert the commit and push. Novinzo keeps only the live version of your theme.

## Keep only what you change

`theme.json` says the theme extends the starter (`"extends": "starter@1.1.0"`). Novinzo
serves every file the theme does not have from the newest starter release of that major
version, so delete the starter's files you have not changed: the pages they render then
follow the starter's fixes and updates without any work on your side. Your own file always
wins over the starter's file of the same path. Don't give a file the name of one of the
starter's in another directory (`snippets/header.liquid` beside the starter's
`sections/header.liquid`): the deploy refuses it, since it would replace the starter's.

Inherited pages use your stylesheet. The deploy writes the starter's Liquid to
`.novinzo/base/` before building, and `assets/input.css` names it as a Tailwind source, so
`theme.css` keeps the classes those pages use.

## Updates to the starter

Once a day, `.github/workflows/starter-updates.yml` asks Novinzo which starter release it
serves. When that is newer than the one the theme extends, it opens a pull request that takes
the release into the starter files you kept, the same day the files you don't keep switch to
it: files you never changed become the release's, and
your changes are merged with the release's. A file where you and the release changed the
same lines is left as it is and listed in the pull request, for you to update by hand.

For it to open pull requests, turn on **Settings → Actions → General → Allow GitHub Actions
to create and approve pull requests**. Close and reopen its pull request to run the check
on it. GitHub pauses scheduled workflows in a public repository after 60 days without
activity; re-enable it from the **Actions** tab.

Novinzo announces breaking changes to the variables templates can read ahead of time, and
flags themes that use them on the dashboard's Theme page.

## License

The theme is [MIT licensed](LICENSE). The Inter and JetBrains Mono fonts in `assets/` are
under the [SIL Open Font License](OFL.txt).
