# 📚 Professional Ultra Pro Reader

**A single-file, zero-dependency web app that reads novels aloud — from your own files or straight off the web.**

Built for long-form reading (web novels, wuxia/xianxia, EPUBs, fan fiction) with synchronized highlighting, Chinese-aware pronunciation, gender-aware translation, and a floating Picture-in-Picture reader.

No build step. No npm. No server. No accounts. **One `.html` file.**

![Single file](https://img.shields.io/badge/single--file-HTML-e8a83e)
![No dependencies](https://img.shields.io/badge/dependencies-0-3fc98e)
![Browser TTS](https://img.shields.io/badge/engine-Web%20Speech%20API-6aa8e8)
![License](https://img.shields.io/badge/license-MIT-blue)

---

## Table of contents

- [Why this exists](#why-this-exists)
- [Features](#features)
- [Quick start](#quick-start)
- [Host it on GitHub Pages](#host-it-on-github-pages)
- [Browser support](#browser-support)
- [Usage guide](#usage-guide)
- [Voice fixes: skip rules & pronunciation](#voice-fixes-skip-rules--pronunciation)
- [Smart Chinese gender translation](#smart-chinese-gender-translation)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Settings reference](#settings-reference)
- [Your data & privacy](#your-data--privacy)
- [Troubleshooting](#troubleshooting)
- [Known limitations](#known-limitations)
- [How it works](#how-it-works)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)

---

## Why this exists

Most read-aloud tools mangle Chinese names, read every `【 】` and `* * *` out loud, call every female character "he", and stop dead at the end of a chapter. This reader fixes all of that:

| Problem | What this app does |
|---|---|
| TTS says "Zee-ao" instead of "Shiao" | 57 built-in pinyin pronunciation rules + syllable splitting |
| Reads decorative symbols aloud | Skip list for words & punctuation (audio only) |
| Every character becomes "he" | Smart Chinese gender detection (她/他 → she/he) |
| Stops at each chapter end | Auto-loads and continues the next chapter |
| Extracted text full of literal `\n` | Automatic escape/entity cleanup into real paragraphs |
| Loses your place | Per-novel folders, auto-saved chapters, bookmarks, resume |

---

## Features

### 📖 Reading & library
- **Folder per novel** — every novel is an expandable folder listing all chapters saved so far, with page and word counts.
- **Auto-save web chapters** — each chapter you read from a website is added to its novel folder automatically (deduplicated by URL).
- **Import** `.txt`, `.md`, `.html`, `.epub` — EPUB is unzipped natively via `DecompressionStream` (no JSZip).
- **Paste text** — drop in a chapter and it's chapter-split and saved.
- **Pagination** (~420 words/page), page list, chapter list, bookmarks with undo, and automatic resume of your last position.
- **Library search & sort** — Recent, Title A–Z, Most chapters; expand/collapse all.

### 🔊 Speech & highlighting
- Browser **Web Speech API** — uses the voices already installed on your device.
- **Five highlight modes**: Word by Word · Entire Sentence/Line · Sentence + Word · Entire Paragraph · Paragraph + Word.
- **Click any word to start reading from that word.**
- **Auto-scroll** that follows the voice and politely pauses when you scroll manually.
- Speed `0.5×–2.5×`, presets, and long-utterance keep-alive so Chrome never stalls mid-chapter.

### 🌐 Website mode
- **Six-source fetch chain** with automatic fallback: direct → AllOrigins → CodeTabs → CorsProxy → AllOrigins JSON → Jina Reader.
- **Reader view** (clean extracted text) or **Live view** (real site in a frame, for signing in).
- **Next/previous chapter detection** from links, `rel`, classes and symbols — plus a URL-number fallback.
- **Auto-continue**: reads to the end, fetches the next chapter, and keeps going until you press stop.
- **Text cleanup** — converts escaped `\n`, `\r\n`, `\uXXXX` and HTML entities into real paragraphs.
- YouTube links open as an embedded player.

### 🈶 Language
- **Auto-translate** into 110+ languages.
- **Smart Chinese gender** — female characters get *she/her*, male characters get *he/him*, with editable per-character overrides.
- **Pronunciation fixes** — pinyin-correct audio, fully editable rules.
- **Skip words / punctuation** — never read aloud, never removed from the page.

### 🖥️ Interface
- **Now-reading HUD**: novel title, chapter, current page, total time and remaining time.
- **Main-screen `+ / −` steppers** for voice speed and text size (also kept in settings).
- **Picture-in-Picture** floating reader with its own controls, highlights and click-to-read.
- **Full screen** in both the main window and the PIP window.
- Dark "lamplight & jade" theme + light paper theme, 9 curated fonts or **any font installed on your device**.

---

## Quick start

1. Download `ProfessionalUltraProReader.html`.
2. Double-click it — it opens in your browser. That's the whole install.
3. Press **▶︎** to hear the bundled sample novel, or paste a chapter URL into **Website → Go → 🔊 Read page**.

> **Recommended:** Chrome or Edge on desktop. Speech synthesis and Picture-in-Picture are best supported there.

Optional local server (avoids any `file://` restrictions):

```bash
python3 -m http.server 8000
# then open http://localhost:8000/ProfessionalUltraProReader.html
```

---

## Host it on GitHub Pages

1. Rename the file to `index.html` and push it to your repo.
2. **Settings → Pages → Source: Deploy from a branch → `main` / `root`.**
3. Your reader is live at `https://<user>.github.io/<repo>/`.

HTTPS hosting also unlocks the **Installed fonts** picker (`queryLocalFonts` requires a secure context).

---

## Browser support

| Capability | Chrome | Edge | Firefox | Safari |
|---|:--:|:--:|:--:|:--:|
| Read aloud (Speech Synthesis) | ✅ | ✅ | ✅ | ✅ |
| Word-level highlighting | ✅ | ✅ | ⚠️ partial | ⚠️ partial |
| Website reader view | ✅ | ✅ | ✅ | ✅ |
| Document Picture-in-Picture | ✅ 116+ | ✅ 116+ | ⚠️ popup fallback | ⚠️ popup fallback |
| EPUB import | ✅ 103+ | ✅ 103+ | ✅ 113+ | ✅ 16.4+ |
| Installed-fonts picker | ✅ | ✅ | ❌ | ❌ |

⚠️ = graceful fallback, not a crash. The app shows a banner if speech synthesis is unavailable.

---

## Usage guide

### Read a web novel
1. Switch to **Website**.
2. Paste a chapter URL → **Go** (loads a clean copy) or **🔊 Read page** (loads *and* starts speaking).
3. Leave **Auto-continue** checked — chapters keep coming and are saved into the novel's library folder.
4. Use **‹ Chapter / Chapter ›** to jump manually.

If a site blocks every source, the reader shows a card with **⟳ Retry**, **🌐 Live view** and **↗ New tab**. Sign in on the live site, or open the tab and paste the text with **📋**.

### Library folders
**📚 Library** → click a folder row to expand it → click any chapter to open it there. **Open** resumes your saved position; **×** deletes the novel (with Undo).

### Picture-in-Picture & full screen
- **🗔 PIP** (or `P`) opens a floating always-on-top reader.
- Inside PIP: **⛶** expands it to fill the screen (native full screen where allowed, otherwise the window is resized and the layout enlarged), **🖥** closes PIP and reads full screen in the main window, **×** closes it.
- **⛶ Full screen** (or `F`) toggles full screen in the main window.

---

## Voice fixes: skip rules & pronunciation

Both live in **⚙ → Voice** and affect **only the spoken audio** — the page text is never altered.

### Skip words / punctuation
Comma-separated words or symbols that are never spoken. Decorative marks are preloaded (`— * • ♪ 【】 「」 《》 ★ → |` …). You can also type names:

```
comma, period, semicolon, colon, dash, quotes, apostrophe,
parentheses, brackets, ellipsis, asterisk, pipe, tilde
```

### Pronunciation fixes
57 editable rules covering the pinyin initials English voices get wrong:

| Pinyin | Spoken as | | Pinyin | Spoken as |
|---|---|---|---|---|
| Xin | Shin | | Qi | Chi |
| Xiao | Shiao | | Qian | Chien |
| Xue | Shue | | Qing | Ching |
| Xuan | Shuan | | Zhang | Jang |
| Xie | Shieh | | Zhou | Joe |
| Xu | Shu | | Cai | Tsai |

Combined names are split into syllables automatically — `Qingcheng → Chingcheng`, `Zhangwei → Jangwei` — while ordinary English words are left untouched.

Add your own (`Yan → Yen`), toggle rules individually, edit, delete with undo, or reset to defaults. The **Test the voice fixes** box shows a live "Will say" preview and speaks it on 🔊.

---

## Smart Chinese gender translation

Mandarin writes 他 and 他/她 with the same sound, so machine translation defaults to "he" for everyone. With **Smart Chinese gender** on (Chinese → English):

1. Character names are detected via a surname table.
2. Gender is inferred from an adjacent **她 / 他**, nearby titles (**师姐, 姑娘, 仙子, 夫人** vs **公子, 师兄, 少爷, 长老**), and gender-typical given-name characters.
3. Names are transliterated, then pronouns in each English sentence are corrected to the nearest character's gender — `he→she`, `him→her`, `his→her`, `himself→herself` (and the reverse for male characters, with possessive/object disambiguation for *her*).

Auto-detected characters are listed in the panel, and manual overrides always win:

```
Lin Xin   → ♀ she / her
Xiao Chen → ♂ he / him
```

---

## Keyboard shortcuts

| Key | Action |
|---|---|
| `Space` | Play / pause |
| `←` / `→` | Previous / next page |
| `S` | Stop |
| `B` | Add bookmark |
| `+` / `-` | Increase / decrease text size |
| `P` | Open Picture-in-Picture |
| `F` | Toggle full screen |
| `Esc` | Close drawer / modal |
| Click a word | Start reading from that word |

Shortcuts are ignored while typing in a field.

---

## Settings reference

**⚙ → Voice**

| Setting | Notes |
|---|---|
| Voice | Any installed voice, or *Auto — match language* |
| Speed | `0.5×–2.5×`, slider + `+ / −` + presets |
| Font | 9 presets, any installed font name, or the font picker |
| Size | `14–32 px`, slider + `+ / −` |
| Highlighting | Word · Sentence · Sentence + Word · Paragraph · Paragraph + Word |
| Auto-scroll | Follows the voice; yields to manual scrolling |
| Clean escaped line breaks | Turns `\n` from web pages into real paragraphs |
| Auto-translate | Off, or 110+ languages |
| Smart Chinese gender | She/her vs he/him correction + overrides |
| Voice language | Auto or forced language for voice selection |
| Skip words / punctuation | Toggle + list + quick-add chips |
| Pronunciation fixes | Toggle + rule list + test preview |

Everything persists automatically.

---

## Your data & privacy

- **No accounts, no analytics, no telemetry, no backend.**
- Library, chapters, bookmarks, progress and settings live in your browser's `localStorage`:
  `pupr.library`, `pupr.bookmarks`, `pupr.progress`, `pupr.settings`, `pupr.lastBook`.
- Clearing site data removes your library — export/import your files if you keep anything irreplaceable.
- `localStorage` is limited to roughly **5 MB** per origin; very long novels may exceed it (the app warns and keeps the book for the session).

**Third-party requests the app can make:**

| Service | When | Purpose |
|---|---|---|
| `api.allorigins.win`, `api.codetabs.com`, `corsproxy.io`, `r.jina.ai` | Loading a web page | Bypass CORS to fetch page text |
| `translate.googleapis.com` | Auto-translate / gender name transliteration | Unofficial translation endpoint |
| `fonts.googleapis.com`, `fonts.gstatic.com` | On load | Web fonts (falls back to system fonts offline) |

Only the URLs and text you ask it to read or translate are sent. Library books read offline make no network requests.

---

## Troubleshooting

**"Failed to load" on a website** — the site blocks reading proxies (Cloudflare, login walls). Try **⟳ Retry**, then **🌐 Live view**, then **↗ New tab** + **📋 Paste text**.

**No voices in the dropdown** — voices load asynchronously; wait a moment or reopen the panel. On Linux, install a speech engine (e.g. `speech-dispatcher` + espeak-ng). Some browsers expose voices only after the first play.

**Highlighting stuck on the first word** — your selected voice doesn't emit word-boundary events. The app falls back to sentence highlighting; switching to a local/native voice usually restores word tracking.

**Text still shows `\n`** — make sure **Clean escaped line breaks** is enabled in ⚙ → Voice, then reload the page.

**PIP full screen does nothing** — Document PiP windows may refuse the native Fullscreen API. **⛶** falls back to resizing the window to your screen and enlarging the layout; **🖥** gives you true full screen in the main window.

**EPUB won't import** — needs `DecompressionStream` (Chrome/Edge 103+, Firefox 113+, Safari 16.4+). DRM-protected EPUBs are not supported.

**Reading stops after ~15 seconds** — fixed by the built-in keep-alive; if it persists, lower the speed slightly or pick a local voice.

**Translation fails** — the unofficial endpoint rate-limits. Wait a bit; untranslated paragraphs fall back to the original text.

---

## Known limitations

- Speech quality and available languages depend entirely on your OS voices.
- Word-level highlighting requires boundary events; not all voices provide them.
- Sites with hard bot protection cannot be extracted — Live view or paste is the workaround.
- Gender inference is heuristic; use overrides for ambiguous or unisex names.
- Pagination is word-count based, not print-accurate.
- No cross-device sync (everything is local to one browser profile).

---

## How it works

- **Extraction** — a miniature readability pass scores candidate containers (`.chapter-content`, `.txt`, `article`, …), serializes block elements and `<br>` into paragraphs, strips chrome/UI lines, and normalizes escapes and entities.
- **Chapter links** — scored from link text, `rel`, class names and arrow glyphs, with a trailing-number URL fallback.
- **Speech pipeline** — text → sentences → ≤230-char units → `speechTransform()` (skip + pronunciation) → utterance. A character map links every spoken index back to its original offset, so highlighting stays aligned even when words are rewritten or removed.
- **Rendering** — paragraphs → sentence spans → word spans with source offsets; highlights are applied by offset lookup, and mirrored into the PIP window.
- **EPUB** — End-of-Central-Directory scan → central directory → `DecompressionStream('deflate-raw')` → `container.xml` → OPF spine → chapter text.

---

## Roadmap

- [ ] Export / import library as JSON (backup & sync)
- [ ] Optional custom proxy / self-hosted fetch endpoint
- [ ] Per-novel voice, speed and pronunciation profiles
- [ ] Sleep timer and skip-back-10s
- [ ] Optional TTS via cloud voices for higher quality
- [ ] Wider gender-aware translation (beyond Chinese → English)

---

## Contributing

Issues and PRs are welcome. Please keep the project's core constraint intact: **one self-contained HTML file, no build step, no runtime dependencies.**

1. Fork and create a branch.
2. Make a targeted change — avoid unrelated reformatting.
3. Test in Chrome/Edge: play/pause, highlighting, click-to-read, auto-scroll, a web chapter with auto-continue, PIP, and full screen.
4. Open a PR describing the behaviour change.

---

## License

MIT — add a `LICENSE` file with your name and year.

The bundled sample novel *The Jade Sword Chronicle* and its Chinese excerpt are original text written for this project and are free to use.

---

<p align="center"><sub>Built for people who'd rather listen to 400 chapters than scroll them.</sub></p>
