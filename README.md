# Aldea IT Solutions — website

A plain static website: hand-written HTML + one CSS file + one small JS file.
No build step, no framework, no dependencies. You can open any `.html` file
in a browser and it works.

## Pages

| File | URL | Purpose |
|------|-----|---------|
| `index.html` | `/` | Home |
| `services.html` | `/services` | Services detail |
| `pricing.html` | `/pricing` | How pricing / quoting works |
| `about.html` | `/about` | Company story + values |
| `support.html` | `/support` | Support request form + FAQ |
| `contact.html` | `/contact` | Enquiry form |
| `terms.html` | `/terms` | Terms and conditions (placeholder — get legal review) |
| `privacy.html` | `/privacy` | Privacy policy (placeholder — get legal review) |
| `thanks.html` | `/thanks` | Shown after a form is submitted |
| `404.html` | — | Shown for unknown URLs |

Shared assets live in `assets/` (`styles.css`, `main.js`, `favicon.svg`).
The header and footer markup is copied into every page — if you change one,
change it in all of them (search for `site-header` / `site-footer`).

`design-source/` holds the original Claude Design export for reference. It is
not part of the deployed site.

## Run it locally

Any static server works. For example, with Node installed:

```bash
npx serve .
```

Then open the URL it prints (usually `http://localhost:3000`).

## Deploy — recommended path: Netlify

Netlify is the easiest option because the **contact and support forms work
automatically** with no third-party signup or API key. Both forms already have
`data-netlify="true"` and a hidden `form-name` field.

### First deploy (drag-and-drop, ~2 minutes)

1. Go to <https://app.netlify.com/drop>.
2. Drag the **whole project folder** onto the page.
3. Netlify gives you a live URL like `random-name-123.netlify.app`.
4. In the site's dashboard: **Forms** — you'll see `contact` and `support`
   after the first real submission.
5. **Forms → Form notifications → Add notification → Email notification** and
   enter the address you want enquiries sent to. Do this for both forms.

### Better: connect it to GitHub so updates auto-deploy

1. Put this folder in a GitHub repo (see "Version control" below).
2. Netlify → **Add new site → Import an existing project → GitHub** → pick the repo.
3. Build command: leave **blank**. Publish directory: `.` (already set in
   `netlify.toml`).
4. Every `git push` now redeploys automatically.

### Custom domain

1. Buy `aldeaitsolutions.com` (or your chosen domain) from any registrar.
2. Netlify → **Domain management → Add a domain** → follow the DNS steps
   (either point your registrar's nameservers at Netlify, or add the CNAME/A
   records Netlify shows you).
3. HTTPS is automatic and free once DNS resolves.

## Deploy — alternative: Cloudflare Pages + Formspree

Cloudflare Pages has no built-in form handling, so the forms need Formspree
(free tier: 50 submissions/month).

1. Create a form at <https://formspree.io> for the address you want mail sent to.
   You'll get a form ID like `abcdwxyz`.
2. In `contact.html` **and** `support.html`, change the `<form ...>` tag:
   - Remove `data-netlify="true"` and `netlify-honeypot="bot-field"`.
   - Set `action="https://formspree.io/f/YOUR_FORM_ID"`.
   - Remove the hidden `<input type="hidden" name="form-name" ...>` line.
   - Optional: add `<input type="hidden" name="_next" value="https://YOURDOMAIN/thanks.html">`
     so it still redirects to the thank-you page.
3. Deploy: Cloudflare dashboard → **Workers & Pages → Create → Pages → Connect
   to Git** (or **Direct Upload** for drag-and-drop). Build command: none.
   Output directory: `/`.

## Version control (optional but recommended)

```bash
git init
git add .
git commit -m "Initial Aldea IT Solutions website"
```

Then create an empty repo on GitHub and follow its "push an existing repository"
instructions. `.gitignore` already excludes editor cruft and the local
`.claude/` folder.

## Things to change before going live

These are placeholders from the design mockup:

- **Email address** — `contact@aldeaitsolutions.com` appears in every page
  footer, on `contact.html`, `terms.html` and `privacy.html`. Replace with a
  mailbox you actually own (or your Gmail for now).
- **Domain** — `aldeaitsolutions.com` appears in `contact.html`, `robots.txt`
  and `sitemap.xml`. Update once the real domain is decided.
- **"40+ projects delivered"** (`index.html`, trust bar) — use a number you can
  stand behind, or reword to something like "Automations, migrations and sites,
  delivered end to end."
- **"38 mailboxes moved to Google Workspace"** and the other hero examples
  (`index.html`) — swap for real examples once you have them, or keep as
  representative "the kind of thing we do" phrasing.
- **"Founded 2026"** (`about.html`) — correct if right.
- **Terms & Privacy** — now written in plain, generic language (`terms.html`,
  `privacy.html`). They cover the basics honestly but are deliberately simple.
  If you ever take on bigger clients, larger contracts, or work across regions
  with strict data rules (GDPR etc.), get them reviewed then.
- **Legal "Last updated" dates** — currently "September 2026". Set to the month
  you actually publish.
```
