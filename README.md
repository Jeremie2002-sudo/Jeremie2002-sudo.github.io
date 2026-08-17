# davidjeremieanand.com

Personal site. One file, no build step, no dependencies.

## Why it's a single file

The whole page is `index.html` — markup and CSS together, no JavaScript, no webfonts, no
external requests of any kind. That is a deliberate performance decision rather than
laziness:

- **One request.** The browser fetches the document and it is done. There is no CSS file to
  block rendering, no font to swap in late and reflow the page, no analytics script.
- **No layout shift.** Type is set in system font stacks, so the first paint is the final
  paint. A webfont would cost an extra request and a visible reflow for a face most readers
  would not consciously notice.
- **The graphics are inline SVG.** The calibrated scales under each figure are a few `<line>`
  elements, which cost less than an image request and stay sharp at any density.

Total weight is roughly 25 KB uncompressed, well under one TCP round trip after compression.

## Local preview

No tooling required — open the file:

```bash
start index.html
```

Or serve it if you prefer a real origin:

```bash
python -m http.server 8000
```

## Deployment

Published with GitHub Pages from the default branch. Pushing to `main` deploys.

### Pointing a custom domain at it

1. Register the domain.
2. At the DNS host, create these records:

   | type | name | value |
   |---|---|---|
   | `A` | `@` | `185.199.108.153` |
   | `A` | `@` | `185.199.109.153` |
   | `A` | `@` | `185.199.110.153` |
   | `A` | `@` | `185.199.111.153` |
   | `CNAME` | `www` | `jeremie2002-sudo.github.io` |

3. Add the domain under **Settings → Pages → Custom domain**. That writes a `CNAME` file to
   the repo.
4. Wait for the certificate to issue, then tick **Enforce HTTPS**.

Do step 3 only after DNS has propagated. Adding the `CNAME` file first makes GitHub redirect
the `github.io` address to a domain that does not resolve yet, which takes the site offline
until DNS catches up.
