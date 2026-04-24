# Bad Decisions Studio Website

Static marketing site for [Bad Decisions Studio](https://www.baddecisions.studio), built around three public paths:
- `Podcast`
- `Learn`
- `Work With Us`

The project is intentionally simple: reusable HTML sections, page templates, one build step, and vanilla CSS/JS.

## Stack

- HTML, CSS, JavaScript
- Vercel for hosting and serverless functions
- Upstash Redis for optional podcast caching
- Self-hosted fonts: `PP Editorial New`, `Inter`, `Azeret Mono`

## Repo layout

```text
Main/
├── build.js                  Build script: composes templates + sections into final pages
├── dev-server.js             Small local static server
├── data/
│   └── site-content.js       Shared content/config for platform links, podcast cards, guests, learn rows
├── templates/                Page templates (head/meta/layout)
│   ├── index.html
│   ├── learn.html
│   ├── podcast.html
│   ├── work-with-us.html
│   └── work-with-us/
│       ├── media-partnerships.html
│       ├── open-roles.html
│       └── services.html
├── sections/                 Source-of-truth HTML partials
│   ├── nav.html
│   ├── hero.html
│   ├── pillars.html
│   ├── find-us.html
│   ├── podcast-landing.html
│   ├── newsletter.html
│   ├── about.html
│   ├── trusted-marquee.html
│   ├── footer.html
│   ├── learn.html
│   ├── podcast.html
│   ├── work-with-us.html
│   └── work-with-us/
│       ├── media-partnerships.html
│       ├── media-partnerships-cta.html
│       ├── open-roles.html
│       └── services.html
├── css/
│   ├── globals.css           Tokens, fonts, base utilities, shared primitives
│   ├── style.css             Section/page styling and responsive rules
│   ├── components.css        Shared component normalization
│   └── nav.css               Shared navigation styling
├── js/
│   └── main.js               Nav, scroll reveal, podcast API refresh, lazy video, mobile sliders
├── api/
│   └── podcast.js            Serverless podcast endpoint
├── assets/
│   ├── bd-logo/
│   ├── client-logos/
│   ├── fonts-web/
│   ├── learn/
│   ├── platform-logos/
│   ├── podcast/
│   └── video/
├── docs/
│   └── AUDIT.md              Historical audit notes
├── index.html                Generated output
├── learn.html                Generated output
├── podcast.html              Generated output
├── work-with-us.html         Generated output
├── work-with-us/
│   ├── media-partnerships.html
│   ├── open-roles.html
│   └── services.html
├── llms.txt
├── sitemap.xml
├── robots.txt
├── vercel.json
├── CLAUDE.md
└── package.json
```

## Build flow

Edit source files only:
- `sections/`
- `templates/`
- `data/site-content.js`
- `css/`
- `js/`

Then rebuild:

```bash
npm run build
```

Do not hand-edit generated pages at the root like:
- `index.html`
- `learn.html`
- `podcast.html`
- `work-with-us/*.html`

They are overwritten by the build.

## Development

Install:

```bash
npm install
```

Build once:

```bash
npm run build
```

Run local server:

```bash
npm run serve
```

Build + serve together:

```bash
npm run dev
```

Default local URLs:
- [http://localhost:4173](http://localhost:4173)
- [http://localhost:8000](http://localhost:8000) when running the simple node server on that port

## Content model

`data/site-content.js` is the canonical source for repeated content:
- platform links/assets/order
- podcast recent episode cards
- podcast guest tiles
- learn free-content rows

If a section uses `{{token}}` placeholders in a partial, the build expands them from `site-content.js`.

## Design system

Shared styling is split intentionally:
- `css/globals.css` for tokens and base primitives
- `css/components.css` for shared component cleanup
- `css/nav.css` for navigation only
- `css/style.css` for page/section styling

Navigation is shared across every page from `sections/nav.html`.

## Environment variables

Optional environment variables for the podcast API:

| Variable | Purpose |
|---|---|
| `YOUTUBE_API_KEY` | YouTube Data API access |
| `KV_REST_API_URL` | Upstash Redis REST URL |
| `KV_REST_API_TOKEN` | Upstash Redis REST token |

If they are missing, the site still renders using static fallback content.

## Deployment

```bash
vercel deploy
vercel deploy --prod
```

Vercel runs `npm run build` through `vercel.json`.

## Repo hygiene

- `.DS_Store`, logs, local env files, and `.claude/` are gitignored
- historical notes live in `docs/` instead of the repo root
- platform links and repeated card content should stay centralized in `data/site-content.js`
