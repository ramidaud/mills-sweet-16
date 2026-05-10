# Sweet 16 Gallery — setup guide

Single-file gallery for client photo delivery. Drop in your photos, push to GitHub Pages, send the link.

---

## Tonight's checklist

- [ ] Export photos from Lightroom into `full/` and `social/` (identical filenames in each)
- [ ] Place `index.html` and `manifest.json` at the repo root, next to those folders
- [ ] Edit `manifest.json` with the real client name and date
- [ ] Update the email address in the `index.html` footer
- [ ] Push the repo to GitHub
- [ ] Enable GitHub Pages: Settings → Pages → Deploy from branch → main → / (root)
- [ ] Wait 1 to 2 minutes, visit your URL, test on phone
- [ ] Send the link to the client

---

## Folder structure

```
.
├── index.html
├── manifest.json
├── full/
│   ├── IMG_0001.jpg
│   └── ...
└── social/
    ├── IMG_0001.jpg
    └── ...
```

**Filenames must match between the two folders.** The gallery loads thumbnails from `social/` and pulls downloads from both.

---

## Lightroom export settings

**`social/` folder (the version visitors see and share):**
- Format: JPEG, sRGB, Quality 80
- Resize: 2048 px on the long edge
- Sharpening: Standard, Screen
- Metadata: All except location

**`full/` folder (the takeaway):**
- Format: JPEG, sRGB, Quality 100
- No resize
- Sharpening: your usual delivery preset
- Metadata: copyright info, no GPS

Export both passes from the same picks so filenames stay aligned.

---

## Generating manifest.json

After exports finish, run from the project root.

**Bash with jq:**

```bash
ls social/ | grep -iE '\.(jpg|jpeg|png)$' | sort | jq -R -s '{client: "Family Sweet Sixteen", date: "May 2026", photos: split("\n") | map(select(length > 0))}' > manifest.json
```

**Python (no dependencies):**

```python
import json, os
files = sorted(f for f in os.listdir('social') if f.lower().endswith(('.jpg','.jpeg','.png')))
manifest = {
    "client": "Family Sweet Sixteen",
    "date": "May 2026",
    "intro": "A selection from the evening. Click any image to view full size and download.",
    "photos": files
}
json.dump(manifest, open('manifest.json','w'), indent=2)
```

---

## Manifest schema

```json
{
  "client": "Display name shown as the page title",
  "date": "Subtitle under the title",
  "intro": "Optional paragraph below the title (omit to use default)",
  "photos": ["IMG_0001.jpg", "IMG_0002.jpg", "..."]
}
```

---

## Customization

- **Client name and date:** in `manifest.json`
- **Color palette:** CSS variables at the top of the `<style>` block in `index.html` (`--bg`, `--accent`, etc.)
- **Typography:** the Google Fonts link in `<head>`, currently Fraunces + DM Sans
- **Footer text and email:** bottom of `<body>`

---

## Notes on GitHub limits

GitHub's 1 GB repo guideline is a soft limit. For one Sweet 16 gallery (around 5 to 7 GB at full res) you may get a warning email but the repo will still work. If client galleries become a recurring thing, switch to **Cloudflare R2 + Cloudflare Pages**: 10 GB free storage, no egress fees, no subscription.

---

## Status

- [ ] Photos edited
- [ ] Photos exported (both sizes)
- [ ] Repo created on GitHub
- [ ] Files pushed
- [ ] Pages enabled
- [ ] Tested on phone
- [ ] Link sent to client
