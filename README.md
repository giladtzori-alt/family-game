# family-game — "מי זה?"

A Hebrew face-recognition game for kids, built from family photos.

**Live:** https://giladtzori-alt.github.io/family-game/

---

## Status (last updated 19 Sep 2026)

| Item | State |
|---|---|
| Game (`index.html`) | ✅ Built, deployed, tested live in both modes |
| 10 face crops | ✅ In `public/family/` |
| `public/family/index.csv` | ⚠️ **Names are broken** — every name cell contains `???` |
| GitHub Pages | ✅ Enabled, `main` / root |
| Repo visibility | Public (needed for free Pages) |

### ⚠️ The one open task

`index.csv` was saved from Excel in a non-Unicode encoding, so the Hebrew names were
destroyed and stored as literal `?` characters. Until it is fixed, every answer button
in the game reads `???`.

**Fix:** in Excel use *Save As → CSV UTF-8 (Comma delimited) (.csv)*, then re-upload the
file to `public/family/`. The orange warning on the game's home screen disappears
automatically once the names load correctly.

---

## How it works

`index.html` is a single self-contained file — no build step, no dependencies, no CDN.
On load it fetches `public/family/index.csv` and builds the deck from it at runtime.

### CSV format

```
ID,name,second name
001,שם,שם שני
002,שם,
```

- `ID` maps to the image: ID `001` → `public/family/face-001.jpg`
- Any column after `ID` is treated as another valid name for that person
- A person is included only if they have an ID and at least one name
- Minimum 3 people required to play (needed for 3 answer options)

### Adding more photos later

1. Crop the face, square, save as `public/family/face-011.jpg`
2. Add a row `011,שם,` to `public/family/index.csv`

That's it — no code changes. The round length grows automatically.

Existing crops are 600×600 JPEG, quality 90, face centred with padding for hair.

---

## Game design (as agreed)

- **Language:** Hebrew, RTL throughout
- **Devices:** phone, tablet, desktop — single screen, no scrolling to answer
- **Mode 1 — בחרו מתוך 3:** photo + three name buttons. Distractors are other people
  whose name differs from the answer; if names are missing or identical it tops up from
  the rest of the pool so there are always exactly 3 options.
- **Mode 2 — אמרו את השם:** adapts to the device.
  - Browsers with the Web Speech API (Chrome, Edge, Android) show a mic button and
    check the spoken Hebrew name.
  - Safari / iPhone / iPad have no such API, so the mic is hidden and the child taps
    *גלו את התשובה* then *ידעתי / לא ידעתי*.
- **Multiple names:** any of a person's names counts as correct. Buttons show the first name.
- **Round:** all photos, shuffled, once each. Score, progress bar, stars, play again.
- Sounds are generated with the Web Audio API — no audio files.

---

## Notes / decisions

- The repo was made **public** deliberately, so GitHub Pages works on a free account.
  This means the ten family face photos are reachable by anyone with the URL.
  `<meta name="robots" content="noindex, nofollow">` keeps them out of search results,
  but it is not privacy. To close this off: make the repo private again (Pages stops
  working) or move hosting elsewhere.
- Face crops were produced with OpenCV Haar cascades (frontal + profile). Two needed
  manual framing: the b&w boy in profile (no detection) and the bonnet close-up
  (detector picked the wrong region).
- `public/family/.gitkeep` is the placeholder that created the empty folder. It can be
  deleted now that the folder has real files.

## Also in this account

`raduil-trip-2026` — separate repo, the Bulgaria trip site, live at
https://giladtzori-alt.github.io/raduil-trip-2026/
