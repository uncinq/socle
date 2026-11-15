---
isIndex: true
title: Home
description: A framework-agnostic CSS foundation, four packages covering tokens, reset, layouts and components.
hero:
  surtitle: CSS foundation
  title: Framework-agnostic packages<br>covering tokens, reset, layouts and components.
  ctas:
    - text: Documentation
      url: /docs/
    - text: GitHub
      url: https://github.com/uncinq/
      blank: true
blocks:
  - type: informations
    ui:
      align: center
      column: 4
      theme: light
    heading:
      surtitle: Framework-agnostic
      title: Works anywhere CSS does
      text: The four packages emit plain CSS and custom properties. No runtime, no preprocessor, no framework binding, so the same entry stylesheet works in every environment.
      ctas:
        - text: Getting started
          url: /docs/getting-started/
          link: true
    items:
      - title: Static site generators
        text: Hugo, Astro, Eleventy, Jekyll. Add the packages to your asset pipeline and import one entry stylesheet.
        icon: zap
      - title: Server-rendered sites
        text: Symfony, Laravel, Django, Rails. Ordinary npm dependencies, bundled like any other stylesheet.
        icon: server
      - title: E-commerce
        text: Shopify, PrestaShop. Custom properties reach a theme without rewriting its templates.
        icon: shopping-bag
      - title: Any build pipeline
        text: PostCSS, Vite, webpack, esbuild. The packages are plain CSS files, with no plugin of their own to install.
        icon: code-xml
  - type: editorial
    ui:
      direction: rtl
    title: "design-tokens"
    text: "Colours, typography, spacing, sizes, radii and motion, as primitive and semantic tokens. Imported once at the top of your entry stylesheet, and the one layer a project overrides to rebrand."
    ctas:
      - text: "View documentation"
        url: "/docs/design-tokens/"
    image:
      src: https://res.cloudinary.com/uncinq/image/upload/v1768396977/593.Drawing-Joy_eulvla.svg
  - type: editorial
    ui:
      theme: light
    title: "component-tokens"
    text: "One namespace per component, from --btn-* to --modal-*. Each maps a semantic value onto a part of a component, so you can restyle buttons alone without moving the brand colour."
    ctas:
      - text: "View documentation"
        url: "/docs/component-tokens/"
    image:
      src: https://res.cloudinary.com/uncinq/image/upload/v1768396979/595.Soup-Tasting_nxgnse.svg
  - type: editorial
    ui:
      direction: rtl
    title: "css-base"
    text: "Reset, native element styles and layout primitives. Unstyled markup already looks right, and every value comes from a token rather than a hardcoded rule."
    ctas:
      - text: "View documentation"
        url: "/docs/css-base/"
    image:
      src: https://res.cloudinary.com/uncinq/image/upload/v1758117379/542.Reading-Expert_brqgji.svg
  - type: editorial
    ui:
      theme: light
    title: "css-components"
    text: "Buttons, cards, navs, alerts, dropdowns, modals and the rest. 29 components and 1 utility, reading their values from component-tokens and adding none of their own."
    ctas:
      - text: "View documentation"
        url: "/docs/css-components/"
    image:
      src: https://res.cloudinary.com/uncinq/image/upload/v1781600882/614.Problem-Solving_nbxuac.svg
---
