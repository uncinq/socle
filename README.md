# Socle

> Documentation site for the CSS foundation.

<img width="1280" height="640" alt="share-socle" src="https://github.com/user-attachments/assets/ed0c4483-a9d6-49f5-99de-ebda466c9f4f" />

**Socle** is French for *foundation*, which is what the four packages it documents are: the ground you build a design system on, not the design system itself.

- [@uncinq/design-tokens](https://github.com/uncinq/design-tokens) — primitive and semantic tokens
- [@uncinq/component-tokens](https://github.com/uncinq/component-tokens) — component-scoped tokens
- [@uncinq/css-base](https://github.com/uncinq/css-base) — reset, native elements, layouts
- [@uncinq/css-components](https://github.com/uncinq/css-components) — component implementations

## How it works

**This repository holds almost no documentation.** Each package carries its own prose in its `docs/` folder and ships it in its npm tarball. The site mounts those folders from `node_modules` and renders them.

Nothing is copied, so the documentation cannot drift from the package it describes. To fix a page, edit it in that package's repository.

The token reference tables are generated the same way, from `dist/tokens.json`, produced by each token package from the same sources as its CSS.

What lives here: the overview and Getting started pages, the `tokens` shortcode, the theme configuration and the branding.

## Development

```bash
yarn install
yarn watch
```

To preview a package's documentation before publishing it, link the local checkout:

```bash
cd ../css-base && yarn link     # once per package
cd ../socle.uncinq.dev && yarn link @uncinq/css-base
```

Run `yarn unlink @uncinq/css-base` to go back to the published version.

| Command | Does |
| --- | --- |
| `yarn watch` | Local server with live reload |
| `yarn build` | Production build |
| `yarn build:search` | Build, then index with Pagefind |

## Deployment

Netlify, from `main`. The build refreshes the four packages before building, and each package release triggers a build hook, so publishing a package republishes the documentation.

`/llms-full.txt` serves the whole documentation as plain text in one request.

## License

MIT © [Un Cinq](https://uncinq.dev/)
