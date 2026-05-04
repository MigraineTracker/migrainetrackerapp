# Migraine Logger — Website

Static site hosted on GitHub Pages. Three pages:

| Path | File | Purpose |
|---|---|---|
| `/` | `index.html` | Marketing landing page |
| `/privacy/` | `privacy/index.html` | Full privacy policy (required by App Store) |
| `/support/` | `support/index.html` | Support page + FAQ (required by App Store) |

Plus `404.html` (custom 404) and `.nojekyll` (tells GitHub to serve files as-is, no Jekyll processing).

No build step. Plain HTML + inline CSS. Edit a file, push, it's live in ~30 seconds.

---

## Option A — Default GitHub Pages URL (free, 5 minutes, recommended)

Repo: **`MigraineTracker/migrainetrackerapp`** (already created at https://github.com/MigraineTracker/migrainetrackerapp).

Resulting URLs once deployed:
- Landing: `https://migrainetracker.github.io/migrainetrackerapp/`
- Privacy: `https://migrainetracker.github.io/migrainetrackerapp/privacy/`
- Support: `https://migrainetracker.github.io/migrainetrackerapp/support/`

The app's [SettingsView.swift](../MigraineTracker/Views/Settings/SettingsView.swift) is already updated to point to these URLs — just push and enable Pages.

### Steps

1. **Push this `website/` folder** to the repo. From the project root, run:

   ```bash
   cd "/Users/suatbatuhanesirger/Downloads/Migraine Tracker Cloude Code/MigraineTracker 34/website"
   git init
   git add .
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/MigraineTracker/migrainetrackerapp.git
   git push -u origin main
   ```

   If GitHub asks for credentials and you don't have a Personal Access Token, easiest path is to install [`gh`](https://cli.github.com/) (`brew install gh`) and run `gh auth login` once, then `git push` works.

2. **Enable GitHub Pages**:
   - Visit https://github.com/MigraineTracker/migrainetrackerapp/settings/pages
   - Under **Source**, choose **Deploy from a branch**
   - Branch: **main**, folder: **/ (root)**
   - Click **Save**
   - GitHub shows the URL within ~30 seconds — should be `https://migrainetracker.github.io/migrainetrackerapp/`

3. **Verify** in a browser:
   - https://migrainetracker.github.io/migrainetrackerapp/ → landing page
   - https://migrainetracker.github.io/migrainetrackerapp/privacy/ → privacy policy
   - https://migrainetracker.github.io/migrainetrackerapp/support/ → support page

   First load can take 1-2 minutes after enabling Pages — if you see a 404, wait and refresh.

4. **In App Store Connect** when you submit, the URLs go in:
   - Privacy Policy URL: `https://migrainetracker.github.io/migrainetrackerapp/privacy/`
   - Support URL: `https://migrainetracker.github.io/migrainetrackerapp/support/`
   - Marketing URL (optional): `https://migrainetracker.github.io/migrainetrackerapp/`

That's it. Free forever, hosted by GitHub, ~99.99% uptime.

### Future updates

When you change a page (typo fix, new policy section, anything):

```bash
cd "/Users/suatbatuhanesirger/Downloads/Migraine Tracker Cloude Code/MigraineTracker 34/website"
# edit a file
git add .
git commit -m "Update privacy section X"
git push
```

Live in ~30 seconds. No build step.

---

## Option B — Custom domain (any `.app` you buy)

If you'd rather the URL be a vanity domain like `https://migrainetracker.app/privacy` (looks more polished, costs ~$15-20/year):

### Costs

- `.app` domain: ~$15-20/year at any registrar (Namecheap, Cloudflare, Porkbun, GoDaddy, etc.)
- GitHub Pages hosting: still free

### Steps

1. **Do all of Option A first** so the site is up at the default URL.

2. **Buy the domain** at a registrar of your choice. `.app` domains require HTTPS (Apple-mandated, free via GitHub Pages anyway).

3. **Add the `CNAME` file** to this folder:

   Create a file named `CNAME` (no extension) in the `website/` folder with one line:
   ```
   your-domain.app
   ```

   Then push:
   ```bash
   git add CNAME
   git commit -m "Custom domain"
   git push
   ```

4. **Configure DNS at your registrar**. Add these DNS records:

   For an apex domain (`your-domain.app`, no www) — add **four A records**:
   ```
   Type   Name   Value
   A      @      185.199.108.153
   A      @      185.199.109.153
   A      @      185.199.110.153
   A      @      185.199.111.153
   ```
   (These are GitHub's official Pages IP addresses.)

   Optionally also add a CNAME for `www`:
   ```
   Type   Name   Value
   CNAME  www    <your-github-username>.github.io
   ```

5. **Configure the domain in GitHub**:
   - Repo → **Settings** → **Pages**
   - Under **Custom domain**, type `your-domain.app` → **Save**
   - Wait 1-30 minutes for DNS to propagate
   - When the green checkmark appears, tick **Enforce HTTPS** (becomes available once cert is provisioned)

6. **Update the app's URL** — change `SettingsView.swift` to point at your custom domain (`https://your-domain.app/privacy/`) and rebuild.

DNS propagation can take a few hours in the worst case but usually within 15 minutes for fresh domains. Once it works, GitHub auto-renews the HTTPS certificate via Let's Encrypt.

---

## Updating the site after launch

Edit any HTML file → commit → push:

```bash
cd "/Users/suatbatuhanesirger/Downloads/Migraine Tracker Cloude Code/MigraineTracker 34/website"
# edit privacy/index.html (or whatever)
git add .
git commit -m "Update privacy policy"
git push
```

Live in ~30 seconds.

When you change the privacy policy *materially* (not typo fixes — changes to data handling), bump the "Last updated" date in `privacy/index.html` and consider showing an in-app notice on next launch (per Apple's guidelines).

---

## What if I'd rather not use GitHub at all?

Any static host works. Drag-and-drop the contents of this folder into:

- **Cloudflare Pages** (free, similar to GitHub Pages)
- **Netlify** (free tier; drag the `website/` folder onto netlify.com/drop)
- **Vercel** (free tier; `vercel deploy` from this folder)

All three give you an HTTPS URL within a minute. Cloudflare Pages is the closest equivalent to GitHub Pages in feature set.

---

## Files in this folder

```
website/
├── .nojekyll              # Tells GitHub Pages: serve files as-is, don't process with Jekyll
├── 404.html               # Shown for missing URLs
├── index.html             # Landing page at /
├── privacy/
│   └── index.html         # Privacy policy at /privacy/
├── support/
│   └── index.html         # Support page at /support/
└── README.md              # This file (not served — GitHub Pages ignores .md at root unless using Jekyll)
```

If you don't want the `README.md` to appear in version control history at all, delete it after reading.
