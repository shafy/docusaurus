---
id: search
title: Search
keywords:
  - algolia
  - search
---

There are a few options you can use to add search to your website:

- 🥇 [Algolia DocSearch](#using-algolia-docsearch) (**official**)
- 👥 [Typesense DocSearch](#using-typesense-docsearch)
- 👥 [Local Search](#using-local-search)
- 👥 [Your own `SearchBar` component](#using-your-own-search)

:::info

🥇 Docusaurus provides **first-class support** for [Algolia DocSearch](#using-algolia-docsearch).

👥 Other options are **maintained by the community**: please report bugs to their respective repositories.

:::

## 🥇 Using Algolia DocSearch {#using-algolia-docsearch}

Docusaurus has **official support** for [Algolia DocSearch](https://docsearch.algolia.com).

The service is **free** in most cases: just [apply to the DocSearch program](https://docsearch.algolia.com/apply).

It works by crawling the content of your website every 24 hours and putting all the content in an Algolia index. This content is then queried directly from your front-end using the Algolia API.

If your website is [not eligible](https://docsearch.algolia.com/docs/who-can-apply) for the free, hosted version of DocSearch, or if your website sits behind a firewall and is not public, then you can [run your own](https://docsearch.algolia.com/docs/run-your-own/) DocSearch crawler.

:::note

By default, the Docusaurus preset generates a [sitemap.xml](https://docusaurus.io/sitemap.xml) that the Algolia crawler can use.

:::

### Index Configuration {#algolia-index-configuration}

After applying, your site's DocSearch config should be created at:

```
https://github.com/algolia/docsearch-configs/blob/master/configs/<indexName>.json
```

This configuration file can be updated by:

- [**asking for help**](#algolia-support): the DocSearch team can help you maintain it
- opening a pull-requests in [algolia/docsearch-configs](https://github.com/algolia/docsearch-configs)

:::caution

It is highly recommended using a config similar to the [**Docusaurus 2 website config**](https://github.com/algolia/docsearch-configs/blob/master/configs/docusaurus-2.json).

:::

### Connecting Algolia {#connecting-algolia}

Docusaurus' own `@docusaurus/preset-classic` supports an Algolia DocSearch integration.

Then, add an `algolia` field in your `themeConfig`. **[Apply for DocSearch](https://docsearch.algolia.com/apply/)** to get your Algolia index and API key.

```jsx title="docusaurus.config.js"
module.exports = {
  // ...
  themeConfig: {
    // ...
    // highlight-start
    algolia: {
      // If Algolia did not provide you any appId, use 'BH4D9OD16A'
      appId: 'YOUR_APP_ID',

      // Public API key: it is safe to commit it
      apiKey: 'YOUR_SEARCH_API_KEY',

      indexName: 'YOUR_INDEX_NAME',

      // Optional: see doc section below
      contextualSearch: true,

      // Optional: Specify domains where the navigation should occur through window.location instead on history.push. Useful when our Algolia config crawls multiple documentation sites and we want to navigate with window.location.href to them.
      externalUrlRegex: 'external\\.com|domain\\.com',

      // Optional: Algolia search parameters
      searchParameters: {},

      //... other Algolia params
    },
    // highlight-end
  },
};
```

:::info

The `searchParameters` option used to be named `algoliaOptions` in Docusaurus v1.

:::

:::caution

The search feature will not work reliably until Algolia crawls your site with the **search plugin enabled**.

If you are installing the Algolia plugin for the first time and want to ensure the search feature works before deploying it to production, you can ask the DocSearch team to trigger a crawl on a staging environment url or deploy preview.

:::

### Contextual search {#contextual-search}

Contextual search is mostly useful for versioned Docusaurus sites.

Let's consider you have 2 docs versions, v1 and v2. When you are browsing v2 docs, it would be odd to return search results for the v1 documentation. Sometimes v1 and v2 docs are quite similar, and you would end up with duplicate search results for the same query (one result per version).

To solve this problem, the contextual search feature understands that you are browsing a specific docs version, and will create the search query filters dynamically.

- browsing `/docs/v1/myDoc`, search results will only include **v1** docs (+ other unversioned pages)
- browsing `/docs/v2/myDoc`, search results will only include **v2** docs (+ other unversioned pages)

```jsx title="docusaurus.config.js"
module.exports = {
  // ...
  themeConfig: {
    // ...
    // highlight-start
    algolia: {
      contextualSearch: true,
    },
    // highlight-end
  },
};
```

:::caution

When using `contextualSearch: true`, the contextual facet filters will be merged with the ones provided with `algolia.searchParameters.facetFilters`.

:::

### Styling your Algolia search {#styling-your-algolia-search}

By default, DocSearch comes with a fine-tuned theme that was designed for accessibility, making sure that colors and contrasts respect standards.

Still, you can reuse the [Infima CSS variables](styling-layout.md#styling-your-site-with-infima) from Docusaurus to style DocSearch by editing the `/src/css/custom.css` file.

```css title="/src/css/custom.css"
html[data-theme='light'] .DocSearch {
  /* --docsearch-primary-color: var(--ifm-color-primary); */
  /* --docsearch-text-color: var(--ifm-font-color-base); */
  --docsearch-muted-color: var(--ifm-color-secondary-darkest);
  --docsearch-container-background: rgba(94, 100, 112, 0.7);
  /* Modal */
  --docsearch-modal-background: var(--ifm-color-secondary-lighter);
  /* Search box */
  --docsearch-searchbox-background: var(--ifm-color-secondary);
  --docsearch-searchbox-focus-background: var(--ifm-color-white);
  /* Hit */
  --docsearch-hit-color: var(--ifm-font-color-base);
  --docsearch-hit-active-color: var(--ifm-color-white);
  --docsearch-hit-background: var(--ifm-color-white);
  /* Footer */
  --docsearch-footer-background: var(--ifm-color-white);
}

html[data-theme='dark'] .DocSearch {
  --docsearch-text-color: var(--ifm-font-color-base);
  --docsearch-muted-color: var(--ifm-color-secondary-darkest);
  --docsearch-container-background: rgba(47, 55, 69, 0.7);
  /* Modal */
  --docsearch-modal-background: var(--ifm-background-color);
  /* Search box */
  --docsearch-searchbox-background: var(--ifm-background-color);
  --docsearch-searchbox-focus-background: var(--ifm-color-black);
  /* Hit */
  --docsearch-hit-color: var(--ifm-font-color-base);
  --docsearch-hit-active-color: var(--ifm-color-white);
  --docsearch-hit-background: var(--ifm-color-emphasis-100);
  /* Footer */
  --docsearch-footer-background: var(--ifm-background-surface-color);
  --docsearch-key-gradient: linear-gradient(
    -26.5deg,
    var(--ifm-color-emphasis-200) 0%,
    var(--ifm-color-emphasis-100) 100%
  );
}
```

### Customizing the Algolia search behavior {#customizing-the-algolia-search-behavior}

<!-- TODO: update options link once the documentation is available on the DocSearch website -->

Algolia DocSearch supports a [list of options](https://autocomplete-experimental.netlify.app/docs/DocSearchModal#reference) that you can pass to the `algolia` field in the `docusaurus.config.js` file.

```js title="docusaurus.config.js"
module.exports = {
  themeConfig: {
    // ...
    algolia: {
      apiKey: 'YOUR_API_KEY',
      indexName: 'YOUR_INDEX_NAME',
      // Options...
    },
  },
};
```

### Editing the Algolia search component {#editing-the-algolia-search-component}

If you prefer to edit the Algolia search React component, swizzle the `SearchBar` component in `@docusaurus/theme-search-algolia`:

```bash npm2yarn
npm run swizzle @docusaurus/theme-search-algolia SearchBar
```

### Support {#algolia-support}

The Algolia DocSearch team can help you figure out search problems on your site.

You can contact them by [email](mailto:documentationsearch@algolia.com) or on [Discord](https://discord.gg/tXdr5mP).

Docusaurus also has an `#algolia` channel on [Discord](https://discordapp.com/invite/docusaurus).

## 👥 Using Typesense DocSearch {#using-typesense-docsearch}

[Typesense](https://typesense.org) DocSearch works similar to Algolia DocSearch, except that your website is indexed into a Typesense search cluster.

Typesense is an [open source](https://github.com/typesense/typesense) instant-search engine that you can either:

- [Self-Host](https://typesense.org/docs/latest/guide/install-typesense.html#option-2-local-machine-self-hosting) on your own servers or
- Use the Managed [Typesense Cloud](https://cloud.typesense.org) service.

Similar to Algolia DocSearch, there are two components:

- [typesense-docsearch-scraper](https://github.com/typesense/typesense-docsearch-scraper) - which scrapes your website and indexes the data in your Typesense cluster.
- [docusaurus-theme-search-typesense](https://github.com/typesense/docusaurus-theme-search-typesense) - a search bar UI component to add to your website.

Read a step-by-step walk-through of how to [run typesense-docsearch-scraper here](https://typesense.org/docs/latest/guide/docsearch.html#step-1-set-up-docsearch-scraper) and how to [install the Search Bar in your Docusaurus Site here](https://typesense.org/docs/latest/guide/docsearch.html#option-a-docusaurus-powered-sites).

## 👥 Using Local Search {#using-local-search}

You can use a local search plugin for websites where the search index is small and can be downloaded to your users' browsers when they visit your website.

You'll find a list of community-supported [local search plugins listed here](https://docusaurus.io/community/resources#search).

## 👥 Using your own search {#using-your-own-search}

To use your own search, swizzle the `SearchBar` component in `@docusaurus/theme-classic`

```bash npm2yarn
npm run swizzle @docusaurus/theme-classic SearchBar
```

This will create a `src/themes/SearchBar` file in your project folder. Restart your dev server and edit the component, you will see that Docusaurus uses your own `SearchBar` component now.

**Notes**: You can alternatively [swizzle from Algolia SearchBar](#editing-the-algolia-search-component) and create your own search component from there.
