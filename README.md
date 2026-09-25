# Useful Finds: static site

Plain HTML + CSS. No build step, no JavaScript, no trackers, no paid services.
Owner: Joshua Ricker · Contact: usefulfindscompany@gmail.com · Handle: @usefulfindscompany

## Files

```
.
├── index.html                                 Home: hero, disclosure, guide links, socials
├── gifts-for-home-cooks-under-50.html         Gift guide (6 products, plain links for now)
├── kitchen-stocking-stuffers-under-25.html    Gift guide (6 products, plain links for now)
├── about.html                                 How products are picked
├── disclosure.html                            Full affiliate disclosure + privacy note
├── 404.html                                   GitHub Pages "not found" page
├── .nojekyll                                  Tells GitHub Pages to serve files as-is
├── assets/
│   ├── styles.css                             All styles (palette + fonts at the top)
│   ├── logo.svg, logo-400.png, logo-96.png    Magnifier-with-star logo (option 1)
│   ├── favicon.svg, favicon-32.png, favicon.ico, apple-touch-icon.png
│   ├── icons/                                 Illustrated category icons used on product cards
│   ├── placeholder-product.svg                Stand-in image for unfinished cards
│   └── social-card-1200x675.jpg               Link-preview image (from the Pinterest cover)
└── screenshots/                               Local preview renders (not needed online)
```

Preview by serving the repository root with a static file server and opening the home page.

## Product cards

Each guide page lists six real products inside `<ul class="product-grid">`. For now they use **plain links** to the brand's official product page (`data-status="plain-link"`), so affiliate programs have a real site to review. A card looks like this:

```html
<li>
  <article class="product-card" data-status="plain-link" data-program="caraway" data-product="KW-KITI" data-brand="Caraway" data-guide="gifts-for-home-cooks-under-50" data-slot="1">
    <div class="product-media">
      <img src="assets/icons/timer.svg" alt="Illustrated icon of a kitchen timer (not a product photo)" width="600" height="600" loading="lazy" decoding="async">
    </div>
    <div class="product-body">
      <p class="product-label">Timer</p>
      <h3>Caraway Mechanical Kitchen Timer</h3>
      <p class="product-desc">One or two sentences on what it's designed to do, based on manufacturer info.</p>
      <a class="btn" href="https://www.carawayhome.com/products/mechanical-kitchen-timer" rel="noopener">Check price<span class="sr-only"> for Caraway Mechanical Kitchen Timer on the Caraway website</span></a>
    </div>
  </article>
</li>
```

### Swapping in an affiliate link (after a program approves you)

1. Replace `href` with the affiliate link from the program's dashboard (Amazon SiteStripe, Impact, Caraway's program, etc.).
2. Change `data-status="plain-link"` to `data-status="affiliate"`.
3. Change `rel="noopener"` to `rel="sponsored nofollow noopener"`.
4. Add `<p class="paid-link">Paid link</p>` right after the button.
5. If the program is different from the one in `data-program` (e.g. you end up linking a Lodge item through Amazon), update `data-program` and the matching row in `../offers.csv`. Set that row's `status` to `affiliate`, and fill in `commission` and `cookie`.

### Adding a new product

1. Copy a whole `<li>...</li>` block and change every field. That includes the name in the `<h3>` **and** in the hidden `sr-only` text inside the button.
2. **Description:** write one or two sentences on what the product is *designed to do*, based only on the manufacturer's page. No "I use this", no ratings or reviews, no health or "non-toxic" claims, no hype words.
3. **Image:** don't hotlink or copy product photos. Use one of the illustrated icons in `assets/icons/` (timer, thermometer, salad-spinner, skillet, utensil-set, strainer, spatula, zester, peeler, paring-knife, pan-scraper, cutting-board), or add a new 600×600 SVG in the same style. If you later want real product images, use only ones you're allowed to use (e.g. Amazon SiteStripe or Product Advertising API images for Amazon links).
4. **Price:** check that the current price is under the guide's budget. Record the price, the date, and the source URL in the `notes` column of `../offers.csv`. Don't show prices on the site.
5. Update the "Last updated" date near the top of the guide.

The grid shows 1 column on phones, 2 on tablets, and 3 on desktop.

To add a new guide (e.g. pantry organization in January), copy one of the guide pages, rename it (e.g. `pantry-organization.html`), change the `<title>`, meta description, `<h1>`, and intro, and link it from the guide grid in `index.html` (replace the "Coming in January" card). Add it to the `<nav>` on every page if you want it in the header.

## Before publishing: checklist

- [x] Social handles confirmed: @usefulfindscompany on Pinterest, TikTok, and Instagram.
- [ ] Re-check that every product is still sold and under budget, and update "Last updated".
- [ ] After approvals, swap plain links for affiliate links (see above). Find remaining plain links with `grep -n 'data-status="plain-link"' *.html`.
- [ ] The Amazon Associates line appears on every page (it's in the footer) and on disclosure.html.
- [x] `og:image` uses https://usefulfindscompany.github.io/assets/social-card-1200x675.jpg on every page.

## Publishing free on GitHub Pages

1. Create a free GitHub account, then create a new **public** repository, e.g. `usefulfinds` (or `USERNAME.github.io` if you want the site at the root URL).
2. Keep these files at the repository root (`index.html` at the top level, not inside a `site/` folder). You can use "Add file → Upload files" in the browser, or:
   ```bash
   git init && git add . && git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/USERNAME/usefulfinds.git
   git push -u origin main
   ```
   You can skip the `screenshots/` folder.
3. In the repo, go to **Settings → Pages**. Under "Build and deployment", set Source to **Deploy from a branch**, Branch to **main**, and folder to **/ (root)**, then save.
4. After a minute or two the site will be live at `https://usefulfindscompany.github.io/`. Settings → Pages shows the exact URL. Tick **Enforce HTTPS**.
5. Optional: a custom domain costs money for the domain itself, but GitHub Pages hosting stays free. Add it under Settings → Pages → Custom domain.
6. Put the site URL in the Pinterest, Instagram, and TikTok profiles, and use it as the website on affiliate program applications. Amazon Associates asks for it at signup.

To update the site later, edit the files, then commit and push again (or upload the changed files). GitHub Pages redeploys automatically.

## Notes

- **Privacy:** the site sets no cookies and has no analytics. Fonts load from Google Fonts, which the privacy note on disclosure.html mentions. To drop that third-party request too, self-host the Fraunces and DM Sans `.woff2` files in `assets/fonts/`, swap the Google `<link>` tags for `@font-face` rules, and update the privacy note.
- **Contrast:** brand terracotta (#C4663F) and sage (#7F8F6E) are too light for small text on cream. Buttons and links use the darker `--rust` (#9A4A2A) and `--sage-dark` (#5C6B4E), which pass WCAG AA.
- **Owner name:** the site doesn't display the owner's name (faceless brand). Add it to about.html or disclosure.html if a program or law requires it.
