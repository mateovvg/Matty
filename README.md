# Matty — The Cute and the Only Cute

A playful, retro-cartoon-poster fan site for **Matty** (Mateo), built as a **single self-contained `index.html`** — HTML, CSS and vanilla JavaScript in one file. No build step, no framework to install: upload it and it runs.

Visual identity: purple checkerboard wall, pink floor, chunky stacked-shadow type, browser-window cards, mascots, sparkles and hand-made cartoon cutouts.

---

## Features

- **Hero** with mascots, floating clouds, sparkles and a scrolling marquee
- **About / Traits** cards styled as retro browser windows
- **Verified Identity Age** card + a hidden-secrets mission ("Discover Truth") that reveals Matty's real age
- **Mini Games** (Snake, memory cards, escape puzzle and more) in a pop-up menu
- **Matty Vibes** and **Best Moments** — a pinned-polaroid photo wall where every photo keeps its own proportions (never cropped)
- **Gallery lightbox** — tap a photo, then browse with arrows, swipe, or the keyboard (← →); wraps around, shows a counter
- **Birthday countdown** to **December 29** — counts to the next birthday automatically every year; on the day it switches to a "Happy Birthday" screen with confetti
- **VIP Supporters** — circular photo cards, each with its own visual theme; cards can be **dragged to reorder**, and whoever sits in the first slot becomes the big featured card; **Nef**'s card plays "Everything Is Awesome" on tap
- **7 character cutouts** placed around the page, each interacting with a real element. Tap one to blow it up into a spotlight with a speech bubble; tap anywhere to close
- **Per-character effects**: every character has its own confetti style and synthesised sound (party poppers, golden stars, camera flash, champagne bubbles, peeking eyes, arcade pixels, falling hearts)
- **Easter egg**: tap the **MATTY** logo 5 times quickly to trigger a "Matty Party" that plays every character's effect in sequence
- **Greeting card maker** — visitors pick a character, an occasion (Birthday, Congrats, Thank you, Good luck, Eid, Just because), optionally add *To* / *From* names and their own message, then **save the card as a PNG** or share it with the phone's native share sheet
- **Visitor counter** — an odometer-style "Matty Fans So Far" counter; each device is counted once
- **Notes Wall** — a moderated guestbook: visitors leave a note, see it as a private preview ("pending review"), and it goes public only after you approve it in a Google Sheet
- **Single yellow loading screen** that waits until every gallery and character image is fully loaded and decoded, then slides away — so images simply *appear* while scrolling
- Responsive on phones, tablets and desktops; respects `prefers-reduced-motion`; keyboard accessible with ARIA labels

---

## Project structure

```
/
├── index.html                     ← the whole site
├── guestbook.gs                   ← Google Apps Script backend for the Notes Wall (paste into Apps Script, not uploaded to the site)
├── README.md
├── images/
│   ├── aura.jpg                   ← gallery photos (see GALLERY)
│   ├── artist-soul.jpg
│   ├── gym-life.jpg
│   ├── cute-essence.jpg
│   ├── model-vibes.jpg
│   ├── matcha-lover.jpg
│   ├── birthday-cake.jpg
│   ├── foodie-moment.jpg
│   ├── coffeeTime.jpg
│   ├── memoryWall.jpg
│   ├── shopping.jpg
│   ├── snipeTime.jpg
│   ├── nef.jpg                    ← supporter photo(s)
│   ├── victory.jpg                ← shown on the win screen
│   └── characters/
│       ├── char-lean.png          ← transparent-background cutouts
│       ├── char-point-up.png
│       ├── char-lookdown.png
│       ├── char-toast.png
│       ├── char-shoulder.png
│       ├── char-point-side.png
│       └── char-heart.png
└── songs/
    └── everythingIsAwesome.mp3    ← played by Nef's card
```

All paths in `index.html` are **relative**, so the folder layout above must be kept exactly (file names are case-sensitive on GitHub Pages).

---

## Run it

- **Locally:** open `index.html` in a browser. (An internet connection is needed for the CDN libraries below.)
- **GitHub Pages:** push the files to your repository → *Settings → Pages* → deploy from the `main` branch (root). Works with a custom domain.
- After uploading a new `index.html`, do a **hard refresh** (or open a private tab) to bypass the browser cache.

### External libraries (loaded from CDNs)

| Library | Used for |
|---|---|
| Tailwind CSS (CDN build) | utility classes / layout |
| Alpine.js 3.13 | all interactivity (loader, games, lightbox, spotlight, supporters) |
| GSAP 3.12 | cursor + small animations |
| canvas-confetti 1.6 | confetti effects |
| AOS 2.3 | scroll-reveal animations |
| Font Awesome 6.4 | icons |

---

## Editing the content

Everything editable is marked with **✏️** in the code. The main settings live in the `<script>` at the bottom of `index.html`.

### Gallery photos — `GALLERY`

```js
{ url: 'images/aura.jpg', title: 'The Aura', desc: 'A presence that commands the room.', w: 1080, h: 1350 },
```

- `url` — path to the photo, `title` / `desc` — caption text.
- `w` / `h` — the photo's real pixel size. These reserve the right amount of space before the image loads, which prevents the page from jumping. Any aspect ratio works; photos are never cropped.

### Character speech bubbles — `CHAR_DATA`

```js
'lean': { img: 'images/characters/char-lean.png', title: 'Welcome! 👋', text: "Hey, I'm Matty! ..." },
```

Keys (`lean`, `point-up`, `lookdown`, `toast`, `shoulder`, `point-side`, `heart`) match the character's CSS class. Edit `title` and `text` freely. Each key's confetti + sound style lives in `CHAR_FX`.

### Birthday — `BIRTHDAY`

```js
const BIRTHDAY = { month: 12, day: 29 };
```

Change the month (1–12) and day. It always counts down to the *next* occurrence, so it never needs updating.

### VIP supporters — `SUPPORTERS`

```js
{ name: 'Name', desc: 'Short description', img: 'images/photo.jpg', theme: 'auto', badge: '⭐' },
```

- **Order = order on the page.** The first entry becomes the big full-width card (visitors can also drag cards live).
- `img: ''` shows a circle with the first letter of the name instead of a photo.
- To hide a supporter without deleting them, put `// ` in front of their line (as done for `lobo`).
- `theme` picks the card style: `goat`, `cosmo`, `chad`, `honda`, `moudir`, `bandar`, `lobo`, `carlos`, `aiden`, `christopher`, `nef`, or `auto` (automatic colour).
- To invent a new look, copy any `.sup-<theme>` CSS block, rename it, and use the new name as `theme`.
- Paste new supporters above the "paste new supporters ABOVE this line" comment, and keep the commas.

### Greeting cards — `CARD_OCCASIONS`

Each occasion has an `id`, a button `label`, a big `head` title (1–2 lines) and a ready-made `line` message. Add or edit entries freely; the card is drawn on a canvas (1080×1350) using the site's own fonts and character images from `CHAR_DATA`.

### Visitor counter — `VISITOR_COUNTER`

```js
const VISITOR_COUNTER = { enabled: true, namespace: 'mateo-matty-site', key: 'visitors' };
```

- Uses the free **Abacus** counting service (`abacus.jasoncameron.dev`); no account needed.
- **Change `namespace`** to your own domain or any name only you use (3–64 letters, numbers, `-`, `_`, `.`), so nobody else shares your counter.
- A device is counted once (remembered in the browser's local storage). If storage is blocked, the number is shown but not increased. If the service is unreachable, the counter shows a friendly "taking a nap" message instead of a fake number.
- Set `enabled: false` to hide the section completely.
- Privacy note: loading the counter sends a request to the Abacus service (like any third-party request, it sees the visitor's IP address).

### Notes Wall — `GUESTBOOK` + `guestbook.gs`

```js
const GUESTBOOK = { url: '', cooldownDays: 7, rejectedShowHours: 24, pageSize: 24 };
```

**How it works**

1. A visitor writes a note (name + message, no links). It appears **only to them** as a preview with a "pending review" badge.
2. The note is saved in your Google Sheet with the status **Pending**, and an email is sent to the address set in `guestbook.gs` (`NOTIFY_EMAIL`).
3. You change the status in column E to **Approved** — the note appears on the public wall right away. Set **Rejected** and the sender sees "wasn't approved" for 24 hours (`rejectedShowHours`), then the note disappears from their device.
4. One note per device every **7 days** (`COOLDOWN_DAYS` in the script and `cooldownDays` on the site).

**Setup (about 10 minutes)**

1. Create a new Google Sheet.
2. *Extensions → Apps Script*, paste the whole of `guestbook.gs`, save.
3. Pick the function `setup` and press **Run** (allow the permissions — Google calls it "unsafe" only because you wrote it). This builds the `Messages` tab with a status dropdown.
4. *Deploy → New deployment → Web app* — Execute as **Me**, access **Anyone** → Deploy → copy the URL ending in `/exec`.
5. Paste it into `GUESTBOOK.url` in `index.html`. Until you do, the section shows a friendly "still being set up" message and the form is disabled.

After editing the script later: *Deploy → Manage deployments → Edit → Version: New version → Deploy* (the URL stays the same).

**Good to know**

- Notes are shown as plain text (never HTML), links are blocked, and a hidden honeypot field plus a site-wide rate limit stop most bots.
- The one-note-per-week limit is tied to the device's browser storage and a device id saved in the sheet (hidden column F); a determined visitor could bypass it by clearing storage.
- Visitors' notes and names are stored in your sheet; mention it in your privacy notice if you publish the site widely.

### Other editable values

- **Ages** for the mission game: `SITE.fakeAge` / `SITE.realAge`.
- **TikTok link:** the `tiktok-inner` link in the About section (`✏️ EDIT: TikTok link`).
- **Marquee words, titles, copy:** plain text in the HTML.
- **Colours:** CSS variables at the top of the `<style>` block (`--purple`, `--pink`, `--orange`, `--yellow`, `--teal`, `--ink`, `--cream`…).

---

## Images — recommendations

- **Gallery photos:** about **1080 px wide**, JPEG quality ~80 %, ideally **under 300 KB each**. Very large originals (3000 px+) are the #1 cause of scroll stutter on phones.
- **Characters:** PNG with transparent background, ~700 px wide.
- **Supporter photos:** square-ish, ~400 px is plenty (they are shown as small circles).

---

## Performance notes

The site is tuned for smooth scrolling on phones:

- The loading screen preloads **and decodes** every gallery/character image before revealing the page.
- Gallery `<img>` tags carry `width`/`height` so no layout shift happens while photos load.
- Background animations (checkerboard, floor) move with `transform` instead of repainting.
- Supporter cards **pause their animations while off-screen**; the pulsing glows become static on touch devices.
- Scroll handlers are throttled to one update per frame and avoid forced layout reads.
- Heavy effects (backdrop blur, tilted/filtered photos, text-gradient animation) are simplified on small screens.
- `prefers-reduced-motion` disables motion and skips confetti.

---

## Accessibility

- Semantic sections with `aria-labelledby`, a "Skip to content" link, and visible focus styles.
- Characters, gallery items and lightbox controls are real buttons with labels; `Esc` closes pop-ups.
- Decorative art is marked `aria-hidden`.

---

## Browser support

Current versions of Chrome, Edge, Safari and Firefox on desktop and mobile. Effects degrade gracefully where a feature is missing (for example, emoji confetti falls back to stars on very old confetti builds).

---

## Credits

Made for **Matty**. Character art and photos belong to their respective owners. "Everything Is Awesome" is © its rights holders — replace `songs/everythingIsAwesome.mp3` with a track you have the right to use if you publish the site publicly.
