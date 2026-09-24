---
isIndex: false
title: Getting started
description: Installing the four packages, declaring the cascade layer order, and the build step every environment needs.
weight: 1
icon: 1-square
---

The setup is the same everywhere: install four npm packages, write one entry stylesheet, run it through PostCSS. Only the last step differs from one environment to the next, and the differences are small.

## 1. Install

```bash
npm install @uncinq/design-tokens @uncinq/component-tokens @uncinq/css-base @uncinq/css-components
```

All four are plain CSS. No runtime, no JavaScript, no framework dependency.

Take only what you need: each package stands on its own as long as the ones below it are present. `css-base` without `css-components` is a reasonable choice; the reverse is not.

## 2. The entry stylesheet

This is the whole integration, and the order is not arbitrary.

```css
/* main.css */
@layer reset, tokens, libs, vendors, base, layouts, components, pages, utilities;

@import '@uncinq/design-tokens';    /* @layer tokens */
@import '@uncinq/css-base';         /* @layer reset, base, layouts */
@import '@uncinq/component-tokens'; /* @layer tokens */
@import '@uncinq/css-components';   /* @layer components, utilities */
```

### Why the layer line comes first

CSS fixes a layer's position **the first time its name is seen**, and later re-declarations do not reorder it. If that line came after an import, the imported package would already have fixed the order and your project could never change it.

None of the four packages declares the order. Each declares only the layers it writes to. Deciding the full order, including the `libs`, `vendors` and `pages` layers that belong to your project and to no package, is your call to make, once, at the top of the entry file.

Read [cascade layers in css-base](../css-base/cascade-layers/) for the detail, including why `libs` and `vendors` are a pair.

### Why tokens come before the CSS that uses them

Custom properties are resolved by the browser at use time, not at import time, so a missing token import raises no error. It silently yields invalid values and unstyled components, which is harder to diagnose than a missing file.

## 3. The build step

Two PostCSS plugins are required, whatever your environment:

```bash
npm install --save-dev postcss postcss-import postcss-custom-media
```

```js
// postcss.config.js
module.exports = {
  plugins: {
    'postcss-import': {},      // resolves @import '@uncinq/...' from node_modules
    'postcss-custom-media': {},// resolves the @custom-media breakpoints
  },
};
```

**Neither is optional.** Without `postcss-import`, the bare package imports do not resolve. Without `postcss-custom-media`, every `@media (--sm)` block is dropped: 26 responsive blocks across `css-base` and `css-components` disappear, no error is raised, and the page renders at its mobile values on every screen.

This is also why a CDN link is not enough for the two CSS packages. `@uncinq/design-tokens` and `@uncinq/component-tokens` emit nothing but custom properties and can be linked directly; `css-base` and `css-components` cannot.

## 4. Wiring it up

The entry stylesheet and the PostCSS config above are shared. What follows is only where each environment expects them.

### PostCSS CLI, no bundler

```bash
npx postcss src/main.css -o dist/main.css
```

### Vite

Put `postcss.config.js` at the project root and import the stylesheet from your entry module. Vite picks the config up on its own.

```js
import './main.css';
```

### webpack

Chain `postcss-loader` after `css-loader` in the rule handling `.css`. The config file is read automatically.

### Hugo

Place the entry stylesheet in `assets/`, then pipe it through PostCSS in your head partial.

```go-html-template
{{ $css := resources.Get "css/main.css" | css.PostCSS | minify | fingerprint }}
<link rel="stylesheet" href="{{ $css.RelPermalink }}">
```

`postcss-import` is what reaches into `node_modules`, so the packages do not need to be mounted into `assets/`.

### Symfony

With Webpack Encore, enable the PostCSS loader and import the stylesheet from your entry:

```js
Encore.enablePostCssLoader();
```

With AssetMapper, compile the stylesheet beforehand and expose the result as a static asset.

### Shopify

Themes do not run a build, so compile the stylesheet in your own pipeline or in CI, commit the result to the theme's `assets/`, and reference it from `theme.liquid`:

```liquid
{{ 'socle.css' | asset_url | stylesheet_tag }}
```

Because everything below the components is custom properties, a theme can then be re-skinned by overriding a handful of tokens, without touching a single Liquid template.

## 5. Verifying

Four checks, in the order things usually break:

1. **Tokens resolve.** Inspect `<html>` in devtools and confirm `--color-brand` has a computed value. If it is empty, the token import is missing or came too late.
2. **Layers are ordered.** The devtools style pane should list your layers in the order you declared. A layer you never declared, appearing last, means something was imported without a `layer()` and is outranking your CSS.
3. **Breakpoints work.** Resize past 768px and confirm a `.row` turns horizontal. If it never does, `postcss-custom-media` is not running.
4. **Components are styled.** Drop a `<button class="btn">` on a page. Unstyled means `@uncinq/component-tokens` is missing: the component CSS is there but has no values to read.

## 6. What to read next

| If you want to | Go to |
| --- | --- |
| Change the brand colour | [design-tokens, customizing](../design-tokens/customizing/) |
| Know what a token is worth | [design-tokens, reference](../design-tokens/reference/) |
| Style a form | [css-base, base](../css-base/base/#forms) and [css-components, forms](../css-components/forms/) |
| Build a modal or a drawer | [css-components, overlays](../css-components/overlays/) |
| Understand the layer order in depth | [css-base, cascade layers](../css-base/cascade-layers/) |
