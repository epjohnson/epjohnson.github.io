# epjohnson.com

A small, hand-coded personal site. The homepage is styled like a 1984 Macintosh System 1 desktop. Projects live under `/projects/`, currently just the Butler Finance research and planner.

## URL targets

- `epjohnson.com` → personal homepage
- `epjohnson.com/projects/` → folder listing all projects
- `epjohnson.com/projects/butler/` → Butler Finance landing page
- `epjohnson.com/projects/butler/planner.html` → interactive planner

## File structure

```
.
├── index.html                  Personal homepage (Mac System 1 aesthetic)
├── README.md                   This file
└── projects/
    ├── index.html              Projects listing
    └── butler/
        ├── index.html          Butler landing / research page
        └── planner.html        Interactive planner
```

## Adding a new project

To add a new project (e.g., a portfolio piece called `atlas`):

1. Create `projects/atlas/` and put the project's files there (typically at minimum an `index.html`).
2. Open `projects/index.html` and find the `<div class="icon-grid">` block. Pick one of the `<span class="finder-icon disabled">` placeholders, change the `<span>` to an `<a>`, remove the `disabled` class, set `href="atlas/"`, and update the label text.
3. (Optional) Update the `info-bar` count from `1 item` to whatever's accurate.
4. Commit and push (or re-upload) — GitHub Pages redeploys automatically.

## Editing the homepage

The homepage is a single self-contained `index.html` at the root. Inline CSS and one inline `<script>` for the menu-bar clock. Only external dependency is Google Fonts (Silkscreen + VT323). Edit the file directly to change copy, add tiles to the Finder window, or wire up new menus.

The "disabled" tiles in the homepage Finder window (Blog, Reading, Music, Garden) can be activated by removing the `disabled` class and setting an `href`.

## Local preview

Just double-click `index.html` and it'll open in your browser. Or run a tiny local server from this folder:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

The local server matters if you want absolute paths to resolve correctly; double-click works fine with the relative paths used here.

## Deployment — GitHub Pages

This site is set up to be deployed via **GitHub Pages**. The flow:

1. Create a public repo named `<your-username>.github.io`.
2. Upload these files to the root of that repo (do NOT nest them in a subfolder).
3. In the repo, **Settings → Pages**, set **Source** to "Deploy from a branch", **Branch** to `main` (folder `/(root)`), then save.
4. Within ~1 minute you'll see "Your site is live at https://`<your-username>`.github.io".
5. To use a custom domain like `epjohnson.com`, see below.

### Custom domain on Namecheap (for `epjohnson.com`)

1. In **Settings → Pages → Custom domain**, enter `epjohnson.com` and save. GitHub will create a `CNAME` file in the repo automatically.
2. In Namecheap → Domain List → Manage on `epjohnson.com` → **Advanced DNS**:
   - **Delete** any existing records with Host `@`, `www`, or `*` that you didn't add yourself (these point to Tumblr / parking).
   - Add four **A Records** with Host `@` and these IPs (these are GitHub Pages' anycast IPs):
     - `185.199.108.153`
     - `185.199.109.153`
     - `185.199.110.153`
     - `185.199.111.153`
   - Add a **CNAME Record** with Host `www`, value `<your-username>.github.io.` (note the trailing dot if Namecheap requires it — usually it doesn't).
   - **Don't** use Namecheap's "URL Redirect Record" for any of these — the routing happens at the DNS / GitHub layer.
3. Save records. DNS propagation usually takes 15–60 minutes.
4. Back in **Settings → Pages**, GitHub will run a DNS check. Once it succeeds, it provisions a free Let's Encrypt SSL certificate (a few more minutes).
5. Tick the **Enforce HTTPS** checkbox once it becomes available.

## Updating the site later

Two ways:

- **Web UI:** in the repo, edit any file directly in the browser (pencil icon in the top-right when viewing a file). Commit. GitHub Pages redeploys in seconds.
- **Drag-and-drop a new file:** click **Add file → Upload files** and drag updates in. Commit.

For larger updates, you can also clone the repo locally with `git`, edit, and `git push`. That's the same workflow — just less browser clicking.

## Troubleshooting

**Site shows a 404 after enabling Pages.** Make sure `index.html` is at the root of the repo (not inside a subfolder). Click around the repo file tree to confirm.

**Custom domain shows "DNS check in progress" forever.** DNS hasn't propagated. Try `dig epjohnson.com` in Terminal or [whatsmydns.net](https://www.whatsmydns.net) to see what the world resolves. Wait longer or double-check the Namecheap A records.

**SSL not available yet.** GitHub provisions Let's Encrypt after DNS resolves. Wait 10 minutes after the DNS check passes, then refresh the Pages settings.

**Old Tumblr content still showing.** DNS cache. Try in an incognito window, or wait.

## Cost summary

| Item                                  | Cost            |
| ------------------------------------- | --------------- |
| GitHub Pages (free for public repos)  | $0              |
| Let's Encrypt SSL (auto via GitHub)   | $0              |
| Namecheap domain renewal              | ~$13/yr         |
| **Total ongoing**                     | $13/yr          |
