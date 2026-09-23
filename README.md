# socle.uncinq.dev

> Documentation site for the Un Cinq CSS foundation.

**Socle** is French for *foundation*. It documents four framework-agnostic packages: [design-tokens](https://github.com/uncinq/design-tokens), [component-tokens](https://github.com/uncinq/component-tokens), [css-base](https://github.com/uncinq/css-base) and [css-components](https://github.com/uncinq/css-components).

## How it works

**This repository holds almost no documentation.** Each package carries its own prose in its `docs/` folder and ships it inside its npm tarball. The site mounts those folders straight from `node_modules` and renders them.

```yaml
# config/_default/module.yaml
mounts:
  - source: node_modules/@uncinq/css-base/docs
    target: content/docs/css-base
```

Nothing is copied or transcribed, so the documentation cannot drift from the package it describes, and it stays readable from a checkout or from `node_modules` without this site.

What does live here: the cross-package content under `content/docs/` (the overview and Getting started), the `tokens` shortcode, and the configuration.

The token reference tables are generated from `dist/tokens.json`, which each token package produces from the same sources and the same serialization function as its CSS.

## Local development

```bash
yarn install
yarn watch
```

To work against your local package checkouts rather than the published versions:

```bash
# once per package
cd ../css-base && yarn link

# in this repository
yarn link @uncinq/css-base
```

Editing a `.md` file in the package repository is then reflected by `hugo server` immediately. Run `yarn unlink @uncinq/css-base` before committing so the site builds against npm again.

## Two configuration rules worth knowing

**Restoring default mounts.** Declaring a mount into `content` or `assets` removes Hugo's default mount for *that component*. `config/_default/module.yaml` restores both explicitly. Removing those two lines empties the site of its own content, silently and without an error.

**Mount sources are directories.** A mount cannot target a single file, which is why the whole of each token package's `dist/` is mounted and the manifest is read with `resources.Get` rather than from `site.Data`.

## Deployment

Netlify, from `main`. The build refreshes the four packages within their `^` ranges before building:

```
yarn upgrade --pattern '@uncinq/' && yarn build:search
```

Each package's release workflow posts to a Netlify build hook (`NETLIFY_DOCS_BUILD_HOOK`), so publishing a package republishes the documentation.

## Scripts

| Command | Does |
| --- | --- |
| `yarn watch` | Local server with live reload |
| `yarn build` | Production build |
| `yarn build:search` | Build, then index with Pagefind |
| `yarn update` | Update the Hugo theme modules |

## License

MIT © [Un Cinq](https://uncinq.dev/)
