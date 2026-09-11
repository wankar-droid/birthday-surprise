# 💜 Birthday Surprise Website

A little interactive birthday surprise: a heart-framed QR code that opens a
website where the person taps an envelope, music starts, and they move
through an intro, a photo scrapbook, a letter, a memory timeline, and a
final birthday message.

## Project structure

```
birthday-surprise/
├── index.html      → the main surprise experience
├── qr.html         → the printable/shareable QR code page
├── style.css       → all styling and animations
├── script.js       → all interactivity + the editable config
├── lib/
│   ├── qrcode.min.js              → self-hosted QR generator (see qr.html)
│   └── qrcode.min.js.LICENSE.txt  → its MIT license
├── assets/
│   ├── music.mp3    → background music (placeholder chime included)
│   ├── photo1.jpg    → placeholder image (replace with real photos)
│   ├── photo2.jpg
│   ├── photo3.jpg
│   ├── photo4.jpg
│   └── photo5.jpg
└── README.md
```

The 5 photo files and the music file already exist as **placeholders** so
you can open the site right away and see the full flow working. Replace
them with your own before sharing it. The `lib/` folder just needs to stay
where it is — it's not something you need to touch.

---

## Step 1 — Add your photos

Replace the 5 files in `assets/` with your own photos, keeping the same
file names:

```
assets/photo1.jpg
assets/photo2.jpg
assets/photo3.jpg
assets/photo4.jpg
assets/photo5.jpg
```

Any image format works as long as you either rename your file to match, or
update the file name in `script.js` (see Step 4). If a photo is missing or
fails to load, the site shows a soft placeholder instead of breaking.

## Step 2 — Add your music

Replace `assets/music.mp3` with your own song, keeping the same file name
(`music.mp3`). If you'd rather use a different file name or format, update
the `music` value inside `birthdayConfig` in `script.js`. If the music file
is missing, the rest of the site still works normally — the music button
simply won't appear.

## Step 3 — Change the person's name

Open `script.js` and find the configuration object near the top:

```javascript
const birthdayConfig = {
  name: "NAME HERE",
  ...
```

Replace `"NAME HERE"` with the person's name. It's used in both the intro
heading and the final "Happy Birthday" message automatically.

## Step 4 — Change the birthday messages

Still inside `script.js`, look for these clearly marked sections:

```javascript
// ===== EDIT THE INTRO MESSAGE HERE =====
introLines: [ ... ]

// ===== EDIT THE BIRTHDAY LETTER HERE =====
letter: `...`

// ===== EDIT THE FINAL MESSAGE HERE =====
finalMessage: `...`
```

You can also edit:
- `photos` — the caption under each scrapbook photo
- `memories` — the 5 memory timeline cards (date label, title, description)
- `wishes` — the short list of wishes shown near the end

Everything on the site pulls from this one `birthdayConfig` object, so you
only need to edit it in this one place.

## Step 5 — Test the website locally

Because the page loads photos and music as files, most browsers want it
served over a local server rather than opened directly as a `file://` URL
(some browsers will still work fine either way, but a local server avoids
any issues).

If you have Python installed, run this from inside the `birthday-surprise`
folder:

```bash
python3 -m http.server 8000
```

Then open **http://localhost:8000** in your browser.

If you have Node.js installed, you can instead run:

```bash
npx serve .
```

Click through the whole flow: envelope → intro → gallery → letter →
timeline → final message → replay, to make sure everything looks right
before sharing it.

## Step 6 — Host the website online

The QR code needs to point to a real, publicly reachable URL — it cannot
point to `localhost` or a `file:///` path, since only your own computer can
open those.

Easiest free options:

**GitHub Pages**
1. Create a new GitHub repository and upload all the files in this folder.
2. In the repository settings, open the "Pages" section and set it to
   deploy from your main branch.
3. GitHub will give you a URL like
   `https://yourusername.github.io/your-repo-name/`

**Netlify**
1. Go to [netlify.com](https://www.netlify.com) and drag-and-drop this
   whole `birthday-surprise` folder onto the dashboard.
2. Netlify will give you a live URL right away.

**Vercel**
1. Go to [vercel.com](https://vercel.com), create a new project, and
   import this folder (or connect it via GitHub).
2. Vercel will give you a live URL.

Any of these work well and are free for a small project like this.

## Step 7 — Change the QR code URL

Once your site is hosted and you have a real URL, open `qr.html` and find:

```javascript
// ===== CHANGE YOUR WEBSITE URL HERE =====
const WEBSITE_URL = "YOUR_WEBSITE_URL_HERE";
```

Replace `"YOUR_WEBSITE_URL_HERE"` with your actual hosted URL, for example:

```javascript
const WEBSITE_URL = "https://yourusername.github.io/your-repo-name/";
```

Then open `qr.html` in a browser, and either:
- Print the page, or
- Display it on another phone/screen for the person to scan

---

## Notes

- The QR code is a normal, fully scannable QR matrix with a decorative
  heart-shaped frame around it — a literal heart-shaped QR *pattern* is
  unreliable for real phone cameras, so readability was kept as the
  priority.
- The site respects `prefers-reduced-motion` and will significantly tone
  down animations for anyone with that setting enabled.
- Everything is built with plain HTML, CSS, and vanilla JavaScript, with
  one small external library (`qrcode.js`, self-hosted in `lib/`, used in
  `qr.html` only) to generate the actual QR code matrix. It's bundled
  locally rather than loaded from a CDN so the page still works offline,
  behind restrictive firewalls, or if a CDN is ever down.
