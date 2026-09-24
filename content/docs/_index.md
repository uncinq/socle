---
isIndex: true
title: Documentation
description: A framework-agnostic CSS foundation, four packages covering tokens, reset, layouts and components.
---

**Socle** is a framework-agnostic CSS foundation, built by Un Cinq. *Socle* is French for foundation, which is what these four packages are: the ground you build a design system on, not the design system itself.

They are deliberately neutral. Nothing here encodes an Un Cinq brand decision beyond a default that you are expected to override, and they work unchanged in Hugo, Symfony, Shopify or a plain HTML page.

## The four packages

| Package | Provides |
| --- | --- |
| [design-tokens](design-tokens/) | Primitive and semantic tokens: colors, typography, spacing, motion |
| [component-tokens](component-tokens/) | Component-scoped tokens for 26 components |
| [css-base](css-base/) | Reset, native element styles, layout primitives |
| [css-components](css-components/) | 29 component implementations and 1 utility |

They stack in that order, each one reading the values the previous one publishes.

```
design-tokens      --color-sienna-600 → --color-brand
component-tokens   --btn-color-background: var(--color-brand)
css-base           styles <button>, <table>, <input> from those tokens
css-components     .btn, .modal, .card and the rest
```

## Start here

New to the stack? Read [Getting started](getting-started/). It covers installing all four, the cascade layer order (the one thing that is genuinely easy to get wrong), and the setups for Hugo, Shopify and Symfony.

Only need one piece? Each package stands on its own, as long as the ones below it are present. `css-base` without `css-components` is a perfectly reasonable choice; the reverse is not.

## How this documentation works

Every page under a package heading is **written in that package's own repository**, in its `docs/` folder, and shipped inside its npm tarball. This site mounts those folders straight from `node_modules` and renders them. Nothing is copied or transcribed.

That has two consequences worth knowing:

The prose sits next to the code it describes, so it is readable offline, from a checkout or from `node_modules`, and it is what an agent working in the repository finds first.

The token references are generated from `dist/tokens.json`, produced by the same build and the same serialization code as the CSS. A token cannot be documented with a value the stylesheet does not ship.

The whole documentation is also available as plain text at [/llms-full.txt](/llms-full.txt), in a single request.
