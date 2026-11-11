# Lellouche site: handoff (2026-11-11)

Self-hosted copy, no more WD (Webdentiste) dependency except the open items below.
Deploy the `public/` folder. `_archive-httrack/` is the raw HTTrack mirror (safe to delete once happy).

## Layout
```
public/
  *.html (15 pages), llms.txt, 5c484d5e433a4cc397d21404a56f5526.txt (IndexNow key, must stay at root)
  blog/index.html, blog/articles/ (3 articles kept)
  assets/css  (theme, elements/, pages/<id>.css, dynamic-styles.css, lellouche-landing.css)
  assets/js   (app-main, app-visitor, theme-*, components/, photoswipe, sharer, vendor/)
  assets/img  (photos/, blog/, filters/, ui/)
```

## Done
- Blog cut to 3 articles + a plain `blog/index.html`; menu "FICHES CONSEILS" renamed "BLOG"; blog sidebar (search/tags) removed.
- Videos removed (menu, player modal, video script).
- WD removed: Matomo tracker, CSRF tags, admin script map, link/phone conversion tracking (disabled), Google Ads loader, hotlinks to WD CDN (all localised).
- Footer links "Hygiène & Asepsie" / "Honoraires & remboursement" removed (articles deleted).
- cgu.html + politique-de-confidentialite.html now use dr.lellouche.patrick@gmail.com (DPO line reduced to that single address).

- Cookie popup (Axeptio) removed from all pages, SDK deleted. GA4 sits in a consent `<template>` that only Axeptio activated, so GA4 is now OFF (Plausible, cookieless, still runs). To re-enable GA4 you need a consent solution first (CNIL).
- Name unified to "Dr Patrick Lellouche" (JSON-LD name + alternateName, og:site_name, header and footer titles).
- sitemap.xml (18 URLs, clean URLs on dr-patrick-lellouche.chirurgiens-dentistes.fr) and robots.txt added; privacy/about/contact.html are redirect stubs.

- Responsive pass: `assets/css/responsive.css` (linked last on every page; delete the link to revert). Justified text now left-aligned everywhere (no stretched word gaps), phone gutters 16px instead of 40-70px, hero shows the whole smile (was a 700px-tall crop), footer menus centred (no staircase), absolute desktop blocks stack on phones, price tables scroll inside their box, body clears the fixed bottom bar.

## TO CLEAN LATER

### Missing files (not in mirror, 404 on WD CDN: get from the Webdentiste admin / originals)
| What | Where it is referenced | Fix |
|---|---|---|
| Image id 10517611 `f5e2d5dc0d855f6e4f7320260720-154811-3u6n6i.jpeg` | `assets/css/pages/1657725.css` (100px bg) | put file in `assets/img/photos/`, update CSS URL |
| Image id 10517613 `2520ed0157031fe9f49520260720-154798-dyomv8.jpeg` | same | same |
| Image id 10517615 `01d53a75c9b91578e72d20260720-154805-f6znfy.webp` | same | same |
| `svg-filters/96848170/1791304290/filter.svg` | `assets/css/pages/1657725.css`, `url(/svg-filters/96848170/...)`, home decorative bg of container 96848170 | save as `assets/img/filters/96848170.svg`, change URL to `../img/filters/96848170.svg` |

Raw list: `missing-refs.txt`. Search both CSS references with `grep -n "10517611\|10517613\|10517615\|96848170" public/assets/css/pages/1657725.css`.

### Still tied to WD / third parties
- **Forms** (`contactez-nous.html`, `prendre-rdv.html`): action posts to `https://dr-patrick-lellouche.chirurgiens-dentistes.fr/api-website-feature/content-forms/…`, a WD backend. Needs own handler (owner will do).
- **mentions-legales.html**: "Éditeur et hébergeur du site" block still names ENPS / info@webdentiste.eu (address, RCS, fax, capital). Rewrite with cabinet as publisher + real host. Also "Responsable de la rédaction : [XX…" placeholder.
- **politique-de-confidentialite.html**: still mentions a HDS-certified hosting subcontractor (WD's); update to the real host.
- **Plausible** (`assets/js/vendor/plausible.js`, key `pa-waT94pCb48KIcNMT-6IZr`): check the accounts belong to the cabinet, not WD. GA4 `G-HPZ47PMVMT` stays.
- `appVisitor.config` in each page (`pagePath`, `sourcePageUrl`, `requestReferer`) still holds old domain values; harmless, remove with the CMS script when ready.

### Not doable on a static host (tracker items)
- Markdown responses for agents (`Accept: text/markdown`), real 301 redirects, root `/` serving French vs English: needs server config.
- Hero image is cropped on phones (background cover, tracker `hero`); needs the original image files.

### Domain / URLs
- sitemap.xml, robots.txt and the privacy/about/contact stubs use the domain dr-patrick-lellouche.chirurgiens-dentistes.fr; change it if the final domain differs. Sitemap uses extensionless URLs (works on GitHub Pages).
- `canonical`, `og:url`, `hreflang` are relative (`index.html`). Make absolute once the final domain is fixed.
- `og:image`, `twitter:image` and JSON-LD images use `https://dr-patrick-lellouche.chirurgiens-dentistes.fr`; change if the domain differs.
- Pages link to `.html`; add clean-URL rewrites if `/le-cabinet` style is wanted (JSON-LD already uses clean URLs).
