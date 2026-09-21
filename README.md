# family-game — "מי זה?"

A Hebrew face-recognition game for kids, built from family photos.

**Live:** https://giladtzori-alt.github.io/family-game/

---

## Status (21 Sep 2026)

| Item | State |
|---|---|
| Game (`index.html`) | ✅ Live and tested |
| People | 18, all with photos |
| Photos | 168 face crops in `public/family/` |
| Round length | 15 photos |
| GitHub Pages | ✅ `main` / root |
| Repo visibility | Public (required for free Pages) |

---

## Data files

Both live in `public/family/` and are read at runtime — **no code changes are needed
to add people or photos.** Save them as **CSV UTF-8**, never plain "CSV", or Hebrew is destroyed.

### `people.csv`

```
id,formal,title,nicknames,pronunciation,gender,spellings
p02,עדו,אבא;דוד,,Iddo,m,עידו
p14,גלעד,סבא,פדרה;סבוש,Gilad,m,
```

| column | meaning |
|---|---|
| `id` | key referenced by `photos.csv` |
| `formal` | the name shown on buttons and on the answer card |
| `title` | `;`-separated. A person can hold several (אמא to her own children, דודה to the rest) |
| `nicknames` | `;`-separated. Shown on the answer card **and** accepted when spoken |
| `pronunciation` | Latin spelling, shown after answering so a child learns how it sounds |
| `gender` | `m` / `f` — drives מי זה vs מי זאת and the feedback wording |
| `spellings` | `;`-separated alternative spellings. **Accepted when spoken, never displayed.** For ktiv male/haser pairs like עדו / עידו |

### `photos.csv`

```
file,person
gilad-01.jpg,p14
```

One row per photo. A person may have any number.

### Adding more

1. Crop the face square, save as `public/family/<name>-NN.jpg`
2. Add a row to `photos.csv`

---

## Game design

- **Hebrew, RTL**, single screen on phone / tablet / desktop
- **One round = 15 photos**, shuffled, preferring a different person for each before repeating anyone
- **Per-question choice.** Each question opens with the photo, מי זה? / מי זאת?, and the
  microphone. The three name buttons are *hidden* behind a quiet
  *לא יודעים? הראו לי 3 אפשרויות* link, so a reading child can't just read the answer.
  A correct answer scores a full ⭐ either way.
- **No microphone (iPhone/iPad):** step 1 shows 🗣️ אמרתי את השם instead, which reveals
  the name and asks ידעתי / לא ידעתי.
- **Microphone test on the home screen** — also gets the browser permission prompt out of
  the way before the first question.
- **Answer card** after every question: title badge, formal name, nicknames, English pronunciation.
- Sounds are generated with the Web Audio API — no audio files.

### Speech matching rules

- Accepted: formal name, any nickname, any alternative spelling, and `title + name` ("דודה יעל")
- A **bare title** counts only when it identifies one person — סבא and סבתא do,
  but אבא / אמא / דוד / דודה do not, otherwise "אבא" would be correct for every father
- The sentence may wrap the name ("זה עידו"), but a fragment of a name is not enough
- Doubled yod/vav collapse (איימי = אימי) and final letters normalise.
  Deliberately **not** stripping all yods/vavs — that would make אור and אייר identical.
- A wrong spoken answer does not end the question; the child can retry or open the options

---

## Notes

- The repo is **public** so Pages works on a free account, which means the face photos are
  reachable by anyone with the URL. `noindex` keeps them out of search results, but that is
  not privacy. To close it: make the repo private (Pages stops) or host elsewhere.
- Crops are 600×600 JPEG q90. Produced with OpenCV Haar cascades using a
  multi-detector vote (a real face is usually found by 2+ detectors; false positives by one).
  Babies defeat the detector often — roughly a third of photos needed a manual box,
  and **every crop was checked visually before upload**.
- HEIC files from iPhone are converted automatically; send them as-is.
- Filenames may carry a position hint when a photo has several people —
  `left`, `right`, `top`, `most left`, `second from left`, `center`. These are parsed
  and used to pick the correct face. **Please keep doing this.**

## Also in this account

`raduil-trip-2026` — the Bulgaria trip site, live at
https://giladtzori-alt.github.io/raduil-trip-2026/
