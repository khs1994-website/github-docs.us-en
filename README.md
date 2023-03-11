# Content <!-- omit in toc -->

The `/content` directory is where all the site's (English) Markdown content lives!

See the [markup reference guide](/contributing/content-markup-reference.md) for more information about supported Markdown features.

See the [contributing docs](/CONTRIBUTING.md) for general information about working with the docs.

- [Frontmatter](#frontmatter)
  - [`versions`](#versions)
  - [`redirect_from`](#redirect_from)
  - [`title`](#title)
  - [`shortTitle`](#shorttitle)
  - [`intro`](#intro)
  - [`permissions`](#permissions)
  - [`product`](#product)
  - [`layout`](#layout)
  - [`children`](#children)
  - [`childGroups`](#childgroups)
  - [`featuredLinks`](#featuredlinks)
  - [`showMiniToc`](#showminitoc)
  - [`allowTitleToDifferFromFilename`](#allowtitletodifferfromfilename)
  - [`changelog`](#changelog)
  - [`defaultPlatform`](#defaultplatform)
  - [`defaultTool`](#defaulttool)
  - [`learningTracks`](#learningtracks)
  - [`includeGuides`](#includeguides)
  - [`type`](#type)
  - [`topics`](#topics)
  - [`communityRedirect`](#communityRedirect)
  - [`effectiveDate`](#effectiveDate)
  - [Escaping single quotes](#escaping-single-quotes)
- [Autogenerated mini TOCs](#autogenerated-mini-tocs)
- [Versioning](#versioning)
- [Filenames](#filenames)
- [Whitespace control](#whitespace-control)
- [Links](#links)
  - [Linking to the current article in a different version of the docs](#linking-to-the-current-article-in-a-different-version-of-the-docs)
  - [Preventing transformations](#preventing-transformations)
  - [Legacy filepaths and redirects for links](#legacy-filepaths-and-redirects-for-links)
  - [Index pages](#index-pages)
  - [Home page](#homepage)
  - [Creating new product guides pages](#creating-new-product-guides-pages)

## Frontmatter

[YAML Frontmatter](https://jekyllrb.com/docs/front-matter/) is an authoring
convention popularized by Jekyll that provides a way to add metadata to pages.
It is a block of key-value content that lives at the top of every Markdown file.

The following frontmatter values have special meanings and requirements for this site.
There's also a schema that's used by the test suite to validate every page's frontmatter.
See [`lib/frontmatter.js`](../lib/frontmatter.js).

### `versions`

- Purpose: Indicates the [versions](../lib/all-versions.js) to which a page applies.
See [Versioning](#versioning) for more info.
- Type: `Object`. Allowable keys map to product names and can be found in the `versions` object in [`lib/frontmatter.js`](../lib/frontmatter.js).
- This frontmatter value is currently **required** for all pages.
- The `*` is used to denote all releases for the version.

Example that applies to GitHub.com and recent versions of GitHub Enterprise Server:

```yaml
title: About your personal dashboard
versions:
  fpt: '*'
  ghes: '>=2.20'
```

Example that applies to all supported versions of GitHub Enterprise Server:
(but not GitHub.com):

```yaml
title: Downloading your license
versions:
  ghes: '*'
```

You can also version a page for a range of releases. This would version the page for GitHub.com, and GitHub Enterprise Server versions 2.22 and 3.0 only:

```yaml
versions:
  fpt: '*'
  ghes: '>=2.22 <3.1'
```

### `redirect_from`

- Purpose: List URLs that should redirect to this page.
- Type: `Array`
- Optional

Example:

```yaml
title: Getting started with GitHub Desktop
redirect_from:
  - /articles/first-launch/
  - /articles/error-github-enterprise-version-is-too-old/
  - /articles/getting-started-with-github-for-windows/
```

See [`contributing/redirects`](../contributing/redirects.md) for more info.

### `title`

- Purpose: Set a human-friendly title for use in the rendered page's `<title>` tag and an `h1` element at the top of the page.
- Type: `String`
- Optional. If omitted, the page `<title>` will still be set, albeit with a generic value like `GitHub.com` or `GitHub Enterprise`.

### `shortTitle`

- Purpose: An abbreviated variant of the page title for use in breadcrumbs and navigation elements.
- Type: `String`
- Optional. If omitted, `title` will be used.

|Article type   |Maximum character length |
---  |  ---  |
|articles	     |     31 |
|categories	  |27 |
|map topics	  |30 |

Example:

```yaml
title: Contributing to projects with GitHub Desktop
shortTitle: Contributing to projects
```

### `intro`

- Purpose: Sets the intro for the page. This string will render after the `title`.
- Type: `String`
- Optional.

### `permissions`

- Purpose: Sets the permission statement for the article. This string will render after the `intro`.
- Type: `String`
- Optional.

### `product`

- Purpose: Sets the product callout for the article. This string will render after the `intro` and `permissions` statement.
- Type: `String`
- Optional.

### `layout`

- Purpose: Render the proper page layout.
- Type: `String` that matches the name of the layout.
For a layout named `components/landing`, the value would be `product-landing`.
- Optional. If omitted, `DefaultLayout` is used.

### `children`

- Purpose: Lists the relative links that belong to the product/category/map topic. See [Index pages](#index-pages) for more info.
- Type: `Array`. Default is `false`.
- Required on `index.md` pages.

### `childGroups`

- Purpose: Renders children into groups on the homepage. See [Homepage](#homepage) for more info.
- Type: `Array`. Default is `false`.
- Require on the homepage `index.md`.

### `featuredLinks`

- Purpose: Renders the linked articles' titles and intros on product landing pages and the homepage.
- Type: `Object`.
- Optional.

The list of popular links are the links displayed on the landing page under the title "Popular." Alternately, you can customize the title "Popular" by setting the `featuredLinks.popularHeading` property to a new string.

Example:

```yaml
featuredLinks:
  gettingStarted:
    - /path/to/page
  startHere:
    - /guides/example
  popular:
    - /path/to/popular/article1
    - /path/to/popular/article2
  popularHeading: An alternate heading to Popular
```

### `showMiniToc`

- Purpose: Indicates whether an article should show a mini table of contents (TOC) above the rest of the content. See [Autogenerated mini TOCs](#autogenerated-mini-tocs) for more info.
- Type: `Boolean`. Default is `true` on articles, and `false` on map topics and `index.md` pages.
- Optional.

### `allowTitleToDifferFromFilename`

- Purpose: Indicates whether a page is allowed to have a title that differs from its filename. For example, `content/rest/reference/orgs.md` has a title of `Organizations` instead of `Orgs`. Pages with this frontmatter set to `true` will not be flagged in tests or updated by `script/reconcile-ids-with-filenames.js`.
- Type: `Boolean`. Default is `false`.
- Optional.

### `changelog`

- Purpose: Render a list of items pulled from [GitHub Changelog](https://github.blog/changelog/) on product landing pages (ex: `components/landing`). The one exception is Education, which pulls from https://github.blog/category/community/education.
- Type: `Object`, properties:
  - `label` -- must be present and corresponds to the labels used in the [GitHub Changelog](https://github.blog/changelog/)
  - `prefix` -- optional string that starts each changelog title that should be omitted in the docs feed. For example, with the prefix `GitHub Actions: ` specified, changelog titles like `GitHub Actions: Some Title Here` will render as `Some Title Here` in the docs feed).
- Optional.

### `defaultPlatform`

- Purpose: Override the initial platform selection for a page. If this frontmatter is omitted, then the platform-specific content matching the reader's operating system is shown by default. This behavior can be changed for individual pages, for which a manual selection is more reasonable. For example, most GitHub Actions runners use Linux and their operating system is independent of the reader's operating system.
- Type: `String`, one of: `mac`, `windows`, `linux`.
- Optional.

Example:

```yaml
defaultPlatform: linux
```

### `defaultTool`

- Purpose: Override the initial tool selection for a page, where the tool refers to the application the reader is using to work with GitHub (such as GitHub.com's web UI, the GitHub CLI, or GitHub Desktop) or the GitHub APIs. For more information about the tool selector, see [Markup reference for GitHub Docs](../contributing/content-markup-reference.md#tool-tags). If this frontmatter is omitted, then the tool-specific content matching the GitHub web UI is shown by default. If a user has indicated a tool preference (by clicking on a tool tab), then the user's preference will be applied instead of the default value.
- Type: `String`, one of: `webui`, `cli`, `desktop`, `curl`, `codespaces`, `vscode`, `importer_cli`, `graphql`, `powershell`, `bash`, `javascript`.
- Optional.

```yaml
defaultTool: cli
```

### `learningTracks`
- Purpose: Render a list of learning tracks on a product's sub-landing page.
- type: `String`. This should reference learning tracks' names defined in [`data/learning-tracks/*.yml`](../data/learning-tracks/README.md).
- Optional

**Note: the featured track is set by a specific property in the learning tracks YAML. See that [README](../data/learning-tracks/README.md) for details.*

### `includeGuides`
- Purpose: Render a list of articles, filterable by `type` and `topics`. Only applicable when used with `layout: product-guides`.
- Type: `Array`
- Optional.

Example:

```yaml
includeGuides:
  - /actions/guides/about-continuous-integration
  - /actions/guides/setting-up-continuous-integration-using-workflow-templates
  - /actions/guides/building-and-testing-nodejs
  - /actions/guides/building-and-testing-powershell
```

### `type`
- Purpose: Indicate the type of article.
- Type: `String`, one of the `overview`, `quick_start`, `tutorial`, `how_to`, `reference`.
- Optional.

### `topics`
- Purpose: Indicate the topics covered by the article. The topics are used to filter guides on some landing pages. For example, the guides at the bottom of [this page](https://docs.github.com/en/actions/guides) can be filtered by topics, and the topics are listed under the guide intro. Topics are also added to all search records that get created for each page. The search records contain a `topics` property that is used to filter search results by topics. For more information, see the [Search](/contributing/search.md) contributing guide. Refer to the content models for more details about adding topics. A full list of  existing topics is located in the [allowed topics file](/data/allowed-topics.js). If topics in article frontmatter and the allow-topics list become out of sync, the [topics CI test](/tests/unit/search/topics.js) will fail.
- Type: Array of `String`s
- Optional: Topics are preferred for each article, but, there may be cases where existing articles don't yet have topics, or adding a topic to a new article may not add value.

### `communityRedirect`
- Purpose: Set a custom link and link name for `Ask the GitHub community` link in the footer.
- Type: `Object`. Properties are `name` and `href`.
- Optional.

### `effectiveDate`
- **For GitHub staff only**: Set an effective date for Terms of Service articles so that engineering teams can automatically re-prompt users to confirm the terms
- Type: `string` YEAR-MONTH-DAY e.g. 2021-10-04 is October 4th, 2021
- Optional.

### Escaping single quotes

If you see two single quotes in a row (`''`) in YML frontmatter where you might expect to see one (`'`), this is the YML-preferred way to escape a single quote. From [the YAML spec](https://yaml.org/spec/history/2001-12-10.html):

> In single-quoted leaves, a single quote character needs to be escaped. This is done by repeating the character.

As an alternative, you can change the single quotes surrounding the frontmatter field to double quotes and leave interior single quotes unescaped.

## Autogenerated mini TOCs

Every article displays a mini table of contents (TOC), which is an autogenerated "In this article" section that includes links to all `H2`s in the article. Only `H2` headers are included in the mini TOCs. If an article uses `H3` or `H4` headers to divide information in a way that only certain sections are relevant to a particular task, you can help people navigate to the content most relevant to them by using a [sectional TOC](../contributing/content-style-guide.md#sectional-tocs).

Mini TOCs do not appear on product landing pages, category landing pages, or map topic pages.

Do not add hardcoded "In this article" sections in the Markdown source or else the page will display duplicate mini TOCs.

## Versioning

A content file can have **two** types of versioning:

* [`versions`](#versions) frontmatter (**required**)
    * Determines in which versions the page is available. See [contributing/permalinks](../contributing/permalinks.md) for more info.
* Liquid statements in content (**optional**)
    * Conditionally render content depending on the current version being viewed. See [contributing/liquid-helpers](../contributing/liquid-helpers.md) for more info. Note Liquid conditionals can also appear in `data` and `include` files.

**Note**: As of early 2021, the `free-pro-team@latest` version is not included URLs. A helper function called `lib/remove-fpt-from-path.js` removes the version from URLs.

## Filenames

When adding a new article, make sure the filename is a [kebab-cased](https://en.wikipedia.org/wiki/Letter_case#Special_case_styles) version of the title you use in the article's [`title`](#title) frontmatter. This can get tricky when a title has punctuation (such as "GitHub's Billing Plans"). A test will flag any discrepancies between title and filename. To override this requirement for a given article, you can add [`allowTitleToDifferFromFilename`](#allowtitletodifferfromfilename) frontmatter.

## Whitespace control

When using Liquid conditionals in lists or tables, you can use [whitespace control](https://shopify.github.io/liquid/basics/whitespace/) characters to prevent the addition of newlines and other whitespace that would break the list or table rendering.

You can add a hyphen (`-`) on either the left, right, or both sides to indicate that there should be no newline or other whitespace on that side.

```
{%- ifversion fpt %}
```

For example, to version a table row, instead of adding liquid versioning for the row starting at the end of the previous row, like this:

```
Column A | Column B | Column C
---------|----------|---------
This row is for all versions | B1 | C1{% ifversion ghes %}
This row is for GHES only | B2 | C2{% endif %}
This row is for all versions | B3 | C3
```

You can include the liquid versioning on its own line and use whitespace control to strip the newline to the left of the liquid tag. This makes reading the source much easier, without breaking the rendering of the table:

```
Column A | Column B | Column C
---------|----------|---------
This row is for all versions | B1 | C1
{%- ifversion ghes %}
This row is for GHES only | B2 | C2
{%- endif %}
This row is for all versions | B3 | C3
```

## Links

Links to docs in the `docs-internal` repository must start with a product ID (like `/actions` or `/admin`) and contain the entire filepath, but not the file extension. For example, `/actions/creating-actions/about-custom-actions`.

Image paths must start with `/assets` and contain the entire filepath including the file extension. For example, `/assets/images/help/settings/settings-account-delete.png`.

The links to Markdown pages undergo some transformations on the server side to match the current page's language and version. The handling for these transformations lives in [`lib/render-content/plugins/rewrite-local-links`](lib/render-content/plugins/rewrite-local-links.js).

For example, if you include the following link in a content file:

```
/github/writing-on-github/creating-a-saved-reply
```
When viewed on GitHub.com docs, the link gets rendered with the language code:
```
/en/github/writing-on-github/creating-a-saved-reply
```
and when viewed on GitHub Enterprise Server docs, the version is included as well:
```
/en/enterprise-server@2.20/github/writing-on-github/creating-a-saved-reply
```

### Using AUTOTITLE for internal links

If you create an internal link, you can use the AUTOTITLE keyword to generate an article's title in the rendered link. See the [markup reference](../contributing/content-markup-reference.md#internal-links-with-autotitle) for details.

### Linking to the current article in a different version of the docs

Sometimes you may want to link from an article to the same article in a different product version. For example:

- You mention some functionality that is not available for free, pro, or team plans and you want to link to the GitHub Enterprise Cloud version of the same page.
- The GitHub Enterprise Server version of an article describes a feature that shipped with that version, but site administrators can upgrade to the latest version of the feature that's in use on GitHub Enterprise Cloud.

You can link directly to a different version of the page using the `currentArticle` property. This means that the link will continue to work directly even if the article URL changes.

```markdown
{% ifversion fpt %}For more information, see the [{% data variables.product.prodname_ghe_cloud %} documentation](/enterprise-cloud@latest/{{ currentArticle }}).{% endif %}
```

### Preventing transformations

Sometimes you want to link to a Dotcom-only article in Enterprise content and you don't want the link to be Enterprise-ified. To prevent the transformation, you should include the preferred version in the path.

```markdown
"[GitHub's Terms of Service](/free-pro-team@latest/github/site-policy/github-terms-of-service)"
```

Sometimes the canonical home of content moves outside the docs site. None of the links included in [`lib/redirects/external-sites.json`](/lib/redirects/external-sites.json) get rewritten. See  [`contributing/redirects.md`](/contributing/redirects.md) for more info about this type of redirect.

### Legacy filepaths and redirects for links

Our docs contain links that use legacy filepaths such as `/article/article-name` or `/github/article-name`. Our docs also contain links that refer to articles by past names. Both of these link types function properly because of redirects, but they are bugs.

When you add a link to an article, use the current filepath and article name.

### Index pages

Index pages are the Table of Contents files for the docs site. Every product, category, and map topic subdirectory has an `index.md` that serves as the landing page. Each `index.md` must contain a `children` frontmatter property with a list of relative links to the child pages of the product, category, or map topic.

**Important note**: The site only knows about paths included in `children` frontmatter. If a directory or article exists but is **not** included in `children`, its path will 404.

### Homepage

The homepage is the main Table of Contents file for the docs site. The homepage must have a complete list of `children`, like every [Index page](#index-page) but must also specify the `childGroups` frontmatter property that will be highlighted in the main content area.

`childGroups` is an array of mappings containing a `name` for the group, an optional `icon` for the group, and an array of `children`.  The `children` in the array must be present in the `children` frontmatter property.

### Creating new product guides pages

To create a product guides page (e.g. [Actions' Guide page](https://docs.github.com/en/actions/guides)), create or modify an existing markdown file with these specific frontmatter values:

1. Use the product guides page template by referencing `layout: product-guides`.
2. (optional) Include the learning tracks in [`learningTracks`](#learningTracks).
3. (optional) Define which articles to include with [`includeGuides`](#includeGuides).

If using learning tracks, they need to be defined in [`data/learning-tracks/*.yml`](../data/learning-tracks/README.md).
If using `includeGuides`, make sure each of the articles in this list has [`topics`](#topics) and [`type`](#type) in its frontmatter.
