---
isIndex: false
title: Getting started
description: Installing the four packages, declaring the cascade layer order, and wiring them into Hugo, Shopify or Symfony.
weight: 1
icon: 1-square
---

## Install

```bash
npm install @uncinq/design-tokens @uncinq/component-tokens @uncinq/css-base @uncinq/css-components
```

All four are plain CSS. There is no runtime, no JavaScript and no framework dependency.

## The entry stylesheet

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

None of the four packages declares the order. Each one declares only the layers it writes to. Deciding the full order, including the `libs`, `vendors` and `pages` layers that belong to your project and not to any package, is your call to make, once, at the top of the entry file.

Read [cascade layers in css-base](../css-base/cascade-layers/) for the detail, including why `libs` and `vendors` are a pair and why an undeclared layer name is dangerous.

### Why tokens come before the CSS that uses them

Custom properties are resolved by the browser at use time, not at import time, so a missing token import does not raise an error. It silently yields invalid values and unstyled components, which is a harder failure to diagnose than a missing file. Import the tokens first.

## The build requirement

`@uncinq/css-base` ships `@custom-media` rules for its breakpoints, which no browser implements natively. Your build must run [postcss-custom-media](https://www.npmjs.com/package/postcss-custom-media).

```bash
npm install --save-dev postcss postcss-custom-media
```

```js
// postcss.config.js
module.exports = {
  plugins: [require('postcss-custom-media')],
};
```

Skipping it fails quietly: every `@media (--sm)` block is dropped, no error is raised, and the page renders at its mobile values on every screen size. If your layouts never respond to width, check this first.

## Hugo

Mount the packages into `assets/`, then build the entry stylesheet through PostCSS.

```yaml
# config/_default/module.yaml
mounts:
  - source: assets
    target: assets
  - source: node_modules/@uncinq/design-tokens/dist/css
    target: assets/css/design-tokens
  - source: node_modules/@uncinq/component-tokens/dist/css
    target: assets/css/component-tokens
  - source: node_modules/@uncinq/css-base/css
    target: assets/css/css-base
  - source: node_modules/@uncinq/css-components/css
    target: assets/css/css-components
```

Remember that declaring a mount into `assets` removes Hugo's default `assets` mount, which is why the first entry restores it.

## Shopify and Symfony

Both consume the packages as ordinary npm dependencies: point your bundler at the entry stylesheet above and ship the result.

The same four packages, the same entry file, the same layer order. That is the reason the packages emit CSS custom properties and nothing else: a JS or SCSS output would have to be regenerated per platform, whereas custom properties are understood everywhere without translation.

If a project needs the token values inside JavaScript, generate that from the same JSON sources rather than hand-maintaining a second list. See [customizing design-tokens](../design-tokens/customizing/#2-json-plus-build).

## Verifying the setup

Four checks, in the order things usually break:

1. **Tokens resolve.** In devtools, inspect `<html>` and confirm `--color-brand` has a computed value. If it is empty, the token import is missing or came too late.
2. **Layers are ordered.** In the devtools style pane, the layer order should match your `@layer` line. If a layer you never declared appears at the end, something was imported without a `layer()` and is outranking your CSS.
3. **Breakpoints work.** Resize past 768px and confirm a `.row` switches to horizontal. If it never does, postcss-custom-media is not running.
4. **Components are styled.** Drop a `<button class="btn">` on a page. Unstyled means `@uncinq/component-tokens` is missing, since the component CSS is there but has no values to read.

## What to read next

| If you want to | Go to |
| --- | --- |
| Change the brand color | [design-tokens, customizing](../design-tokens/customizing/) |
| Know what a token is worth | [design-tokens, reference](../design-tokens/reference/) |
| Style a form | [css-base, base](../css-base/base/#forms) and [css-components, forms](../css-components/forms/) |
| Build a modal or a drawer | [css-components, overlays](../css-components/overlays/) |
| Understand the layer order in depth | [css-base, cascade layers](../css-base/cascade-layers/) |
