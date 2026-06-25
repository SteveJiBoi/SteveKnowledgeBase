# Knowledge Hub

A self-hostable, static website that turns a folder of HTML study pages into a searchable library. Filter by subject label, preview any topic in-page, and upload new HTML files — either instantly in your browser or permanently into your GitHub repo.

Everything is plain static files, so it runs perfectly on **GitHub Pages** with zero backend.

```
knowledge-hub/
├── index.html        ← the site (search, filter, preview, upload) — all in one file
├── manifest.json     ← the "database": the list of topics + their labels
├── content/          ← the actual HTML pages
│   ├── ap-physics-c-em-unit-1-review.html
│   ├── ap-physics-c-em-unit-2-review.html
│   ├── calculus-for-ml-review.html
│   └── linear-algebra-for-ml-review.html
└── README.md
```

---

## 1. Put it on GitHub Pages (10 minutes, free)

1. Create a new repository on GitHub, e.g. **`knowledge-hub`** (public is simplest).
2. Upload **everything inside this `knowledge-hub` folder** to the repo root — `index.html`, `manifest.json`, the `content/` folder, and this README. (Drag-and-drop works at *github.com → your repo → Add file → Upload files*.)
3. In the repo, go to **Settings → Pages**.
4. Under **Build and deployment → Source**, pick **Deploy from a branch**, choose branch **`main`** and folder **`/ (root)`**, then **Save**.
5. Wait ~1 minute. Your site is live at:

   ```
   https://<your-username>.github.io/knowledge-hub/
   ```

That URL is your hub. The four starter topics appear right away.

> Tip: open `index.html` by visiting that URL — **not** by double-clicking the file. Browsers block local files from reading `manifest.json`, so the page needs to be served over http(s). (To preview locally, run `python3 -m http.server` inside this folder and open `http://localhost:8000`.)

---

## 2. Using the site

- **Search** — type in the search box to filter topics by title, description, or label.
- **Filter by label** — click the subject chips (All / Physics / Mathematics / …). Click several to combine them.
- **Preview** — click any card (or its **Preview** button) to read the topic full-screen, without leaving the hub. **Open full page ↗** opens it in its own tab.
- **Dark / light** — the ☼ button toggles the theme; your choice is remembered.

---

## 3. Uploading new topics

Click **Upload**, choose an `.html` file, give it a title + description, tick one or more **labels** (or type a new label and hit **Add**), then pick where to save:

### Option A — "This browser" (no setup)
The file is stored in *this browser only* (via IndexedDB). Great for a quick look. It shows a **Browser only** badge and is **not** saved to GitHub or visible on other devices. Use the **Remove** button on the card to delete it.

### Option B — "GitHub (permanent)" — recommended
The upload is committed straight into your repo's `content/` folder **and** `manifest.json` is updated, in a single commit. Within ~1 minute GitHub Pages rebuilds and the topic is live for everyone, on every device. This needs a one-time token (next section).

---

## 4. One-time GitHub token setup (for permanent uploads)

This lets the **Upload** button write to your repository.

1. Go to **GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens** → **Generate new token**.
   (Direct link: https://github.com/settings/tokens?type=beta )
2. Set:
   - **Token name**: `knowledge-hub`
   - **Expiration**: your choice (e.g. 90 days)
   - **Repository access**: **Only select repositories** → choose your `knowledge-hub` repo.
   - **Permissions → Repository permissions → Contents**: **Read and write**.
3. **Generate token** and copy it (starts with `github_pat_…`).
4. On your hub site, click the **⚙ gear** (top right) and fill in:
   - **Owner** = your GitHub username
   - **Repository** = `knowledge-hub`
   - **Branch** = `main`
   - **Content folder** = `content`
   - **Personal access token** = paste it
   
   (When the site is hosted on `github.io`, owner and repo are auto-filled.)
5. Click **Test connection** → you should see ✓ Connected. Then **Save**.

Now the **GitHub (permanent)** option is enabled in the Upload dialog.

### Is the token safe?
- It is stored **only in your browser's local storage** — never sent anywhere except GitHub's API.
- It's **fine-grained** and limited to this one repo with only **Contents** access, so the worst case is limited to this repository.
- Use the gear's **Forget token** button to remove it (e.g. on a shared computer), and revoke it anytime on GitHub.

---

## 5. Editing topics by hand (optional)

`manifest.json` is just a list — you can edit it directly in GitHub:

```json
{
  "labels": ["Physics", "Mathematics", "Computer Science"],
  "documents": [
    {
      "id": "calculus-for-ml",
      "title": "Calculus for Machine Learning",
      "description": "Derivatives, gradients & gradient descent.",
      "file": "content/calculus-for-ml-review.html",
      "labels": ["Mathematics"],
      "added": "2026-06-24",
      "size": 140295
    }
  ]
}
```

To add a topic manually: drop the `.html` into `content/`, add a matching block to `documents`, and commit. To rename a label everywhere, change it under `labels` and in each document's `labels`.

---

## FAQ

**Do uploaded files really persist on GitHub Pages?** Yes — with the token (Option B). GitHub Pages can't accept uploads on its own (it's static), so the site uses GitHub's API to commit the file for you. Without a token, uploads are browser-only (Option A).

**My uploaded HTML didn't appear immediately on the live site.** GitHub Pages takes ~30–60s to rebuild after each commit. The hub shows it instantly for your current session; a refresh after a minute pulls it from the repo.

**Will big files work?** Yes — the starter physics pages are ~2.5 MB each and upload fine. Keep individual files under ~25 MB to be safe.
