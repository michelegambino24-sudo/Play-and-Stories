# Play & Stories — SPEC

Working name: **Play & Stories**. This file is the source of truth for what we build. AGENTS.md says how Codex works on it.

---

## 1. What this is

A small static website with two playful tools for young children and their parents. It is a **concept demo** that Michele shows to his managers at Strategy& to illustrate what a delightful, simple digital product around early childhood could feel like.

- It is **not** a real product of any government entity, and must never look like one.
- It takes **inspiration** (colours, tone, layout feel) from the Abu Dhabi Early Childhood Authority website, but copies nothing: no logo, no name, no illustrations, no text.
- Success = a manager opens it on a phone or tablet, plays for 2 minutes, smiles, and understands the idea ("play is learning", "stories at bedtime").

## 2. Hard rules (never break)

1. **Static only.** Plain HTML, CSS, JavaScript. No backend, no framework, no build step, no runtime npm dependencies. Hosted free on GitHub Pages.
2. **Works two ways:** by double-clicking `index.html`, and on GitHub Pages under `/<repo-name>/`. So: relative paths only (never a leading `/`), classic `<script>` tags (no ES modules), no `fetch()` of local files. Content libraries are `.js` files that define globals.
3. **Simulation only.** No accounts, no logins, no real children's names or data, no forms that collect anything, no cookies, no analytics, nothing sent anywhere. No external requests of any kind: no CDNs, no Google Fonts, no embeds. The only thing that may be stored (localStorage) is the chosen language.
4. **Free.** No paid services, no API keys, no AI calls, no text-to-speech services.
5. **No copyrighted images.** All visuals are original SVG written by us (or CSS shapes). No stock photos, no traced or imitated characters, no ECA artwork.
6. **Clearly a concept.** Every screen shows a visible badge: "Concept demo — not an official product". The landing page also says it in one plain sentence. No claim that ECA, the government or any expert endorses the content.
7. **Arabic is a draft** until a native speaker reviews it (see section 9). The UI must say so whenever Arabic is shown.
8. **Safety wording.** Activities involving water, small parts or climbing must say adults stay close. Nothing is presented as medical or developmental advice: "general play ideas".

## 3. Design language

Taken from the ECA screenshots by eye (approximate; sample the real values from the screenshots in `docs/reference/` if possible):

| Role | Colour |
|---|---|
| Coral (main background, hero) | `#F26649` |
| Navy (text on light, dark sections, frames) | `#1F1F5B` |
| Sky blue | `#83CAE8` |
| Sunshine yellow | `#FDD83E` |
| Pink | `#EBA3B2` |
| Peach | `#FBAE78` |
| Teal green | `#5A9684` |
| Mist (light section background) | `#E6EEED` |

Look and feel to echo:
- Flat illustration, no gradients, no shadows-as-decoration. Big organic **blob shapes** behind content.
- **Hand-drawn touches**: wobbly-edged picture frames, loose circle outlines, a wavy line connecting stages.
- A **journey strip**: coloured blobs joined by a line (use it for the age picker in Play Spinner: 0 to 8).
- Heavy, rounded, geometric sans-serif headings, large and friendly; very little text.
- Hero layout idea: big title and one short sentence on the left, a framed illustration on the right, blobs behind.

Our own identity (so it is not a copy):
- Our own mark: a simple original shape logo for "Play & Stories" (for example a spinning-wheel or moon-and-blob mark), not resembling the ECA logo.
- Our own characters and scenes. Different shapes, different compositions.
- Do not reproduce the ECA illustration of a family or any specific picture from their site.

Contrast: white text on coral fails WCAG AA for normal text. Use navy text on coral, or white only for very large bold text (check ratios). Every text/background pair must pass AA.

Typography: system font stack, or a self-hosted open-licence font file committed in the repo with its licence text (ask in the plan before adding one). Arabic needs its own font stack for good shaping.

## 4. Audience and interaction principles

- Two audiences: **parent** (Play Spinner) and **child with a parent** (Story Builder).
- Big tap targets: minimum 56 px, 72 px or more for children's picture cards.
- Very little text. Short words, one idea per screen. Icons carry meaning together with labels (never icon alone).
- Works on phone (360 px wide) and tablet, portrait and landscape; desktop is fine but secondary.
- Keyboard and screen-reader usable: real `<button>`s, visible focus, labels, `lang` and `dir` set correctly, live region announcing the spin result and the story.
- `prefers-reduced-motion`: no spinning, bouncing or parallax; replace with instant or fade-only changes. Provide the same information.
- No timers, no flashing, no sound by default.

## 5. Screens and flow

```
Landing  ->  Choose a game  ->  Play Spinner   (setup -> spin -> result -> again / home)
                            ->  Story Builder  (hero -> place -> problem -> story -> goodnight / again / home)
```

Every page has: a "Home" button, the language switch (English / العربية), and the concept-demo badge.

### 5.1 Landing
Hero with the working name, one line ("Little games for play and bedtime"), one big "Let's play" button, the concept-demo sentence, and the journey strip of age blobs as decoration. ECA-inspired, not ECA.

### 5.2 Choose a game
Two huge cards: **Play Spinner** (for parents) and **Bedtime Story Builder** (for children with a parent). Each with an illustration, a 2-word title and a one-line description.

### 5.3 Game 1: Play Spinner (for parents)
Message: **"Play is learning."**

Inputs (one step per screen on phone):
1. Child's age, 0 to 8 (journey-strip blobs).
2. Time available: 10 minutes / 30 minutes / an afternoon.
3. Indoors or outdoors.
4. Materials at home (multi-select, icon + label): boxes, water, paper, blocks, fabric, kitchen things, nature (sand, stones, leaves), music-makers (pots, spoons), and "nothing special".

Then: spin a wheel, get one activity.

Result card shows: title, icon illustration, "develops" chips (Language, Motor skills, Emotions, Thinking; one or two), a **3-step how-to** (one short sentence each), time and materials, a one-line "why it helps", and the safety line. Buttons: Spin again, Change my answers, Home.

Selection logic:
- Hard filters: age inside the activity's range; activity time less than or equal to the time chosen (10 < 30 < afternoon); setting matches or activity is "both"; every material the activity needs is among the chosen materials or "none needed".
- Wheel shows up to 6 matching candidates; the winner is random among them; "Spin again" excludes ideas already shown until the pool is empty.
- If fewer than 3 matches, relax in this order: time, then materials, then setting; and tell the user in one plain sentence that these are the "closest ideas". Never show an empty result.
- Reduced motion: no spin animation, a "Pick for me" button reveals the result directly.

**Activity library: 40 to 60 activities** in `js/data/activities.js`. Schema:

```js
{
  id: "box-boat",
  ageMin: 2, ageMax: 5,
  time: 30,                       // 10 | 30 | 180 (afternoon)
  setting: "indoors",             // "indoors" | "outdoors" | "both"
  materials: ["boxes"],           // [] means none needed
  develops: ["thinking", "motor"],// language | motor | emotions | thinking
  icon: "box",                    // key into the SVG icon set
  title:   { en: "Box boat" },
  why:     { en: "..." },
  steps:   { en: ["...", "...", "..."] },
  safety:  true                   // show the adults-stay-close line
}
```
Arabic strings are added later (Step 4) under `ar` keys; English must work with no `ar` present.

Coverage targets (checked by a script): every age 0 to 8 has at least 4 activities; indoors at least 18, outdoors at least 18; each time length at least 10; each "develops" tag at least 10; at least 8 activities need no materials; each material appears in at least 3 activities. Ideas must be realistic for a Gulf home (heat means indoors midday; shade; cushions; dates and kitchen items; sand, shells, palm leaves) without stereotypes, religious content or brand names. Babies (0 to 1): sensory and face-to-face play only, nothing with small parts.

### 5.4 Game 2: Bedtime Story Builder (child with a parent)
The child taps big picture cards, in this order:
1. **Hero** (8): falcon, camel, robot, girl astronaut, fennec fox, sea turtle, Arabian oryx, boy with a kite.
2. **Place** (8): desert dunes, souq, sea shore, space, oasis, mountain, city at night, garden.
3. **Problem** (6): lost something, can't sleep, big wind, a friend is sad, a locked door, something broken.

(These lists are proposals; the Step 3 plan may adjust them.)

Then the page assembles a **short story to read aloud**: 3 illustrated panels, each with 2 to 3 short sentences (large text, about 12 words per sentence), always ending warm and sleepy, then a **Goodnight** screen (moon, dimmed colours, no motion).

Variety: at least **5 structural templates** (for example: lost-and-found, helping a friend, the big wind, the mystery sound, the small challenge). Template choice depends on the problem plus randomness; sentences have slots for hero, place and the problem object. Hero, place and problem each carry names and small phrase variants so 8 x 8 x 6 combinations do not feel identical. A "Tell it again" button re-rolls the template for the same three cards.

Illustrations: each panel is composed from original SVG layers (place backdrop + hero figure + problem prop), so any combination renders without extra art.

Arabic: every story string exists in English and Arabic. The language switch flips the whole page, including `dir="rtl"`, mirrored layout and text alignment. Grammatical gender: each hero has a gender key, and Arabic templates must use it (verbs and adjectives agree with the hero). Decide the data shape in the Step 3 plan so Step 4 only adds Arabic text, not new logic.

## 6. Tech and file layout

```
index.html            landing
games.html            choose a game
spinner.html          Play Spinner
story.html            Bedtime Story Builder
css/                  base.css (tokens, reset, type), components.css, plus per-page files
js/common.js          language switch, badge, shared helpers
js/i18n.js            UI strings (en, later ar)
js/spinner.js, js/story.js
js/data/activities.js, js/data/stories.js
assets/               original SVG files (logo, icons, characters, backdrops) and any self-hosted font with licence
docs/reference/       ECA screenshots (inspiration only, never published in the UI)
tests/check-data.js   zero-dependency Node script validating the libraries
SPEC.md, AGENTS.md, README.md, ARABIC-REVIEW.md (Step 4)
```
Inline SVG or `<img src>` with relative paths are both fine. Keep each page light (target under 300 KB without fonts).

## 7. Testing

- `node tests/check-data.js` (no dependencies): validates schema and coverage targets of the activity library and story data; prints a clear pass/fail. Added in Step 2, extended in Step 3 and 4.
- Manual checklist at the end of every step, written by Codex in its final message: 360 px phone, tablet landscape, keyboard-only pass, reduced motion on, no console errors, no network requests (browser devtools Network tab empty apart from local files).
- No test may hit the network.

## 8. Build steps (one step per Codex session)

**Step 1 — Landing page + game choice screen.** Design tokens and base CSS, shared header/badge/language-switch skeleton (English only active), original logo mark, landing page, choose-a-game page with two cards linking to placeholder pages for the games. Done when: both pages match the ECA-inspired language without copying it, the concept badge shows everywhere, 360 px and tablet look right, contrast and focus are checked, reduced motion respected, no external requests.

**Step 2 — Play Spinner.** Inputs, wheel, result card, selection logic with relaxation, library of 40 to 60 activities, icon set, `tests/check-data.js`. Done when: the coverage targets pass, every age 0 to 8 returns a result for common answers, reduced-motion path works, safety line shows where needed.

**Step 3 — Story Builder (English).** Cards for hero, place and problem, SVG layers, at least 5 templates, 3-panel story, tell-it-again, goodnight screen. Done when: all 384 combinations render a complete story with no leftover slots or empty text (script check), and it is readable aloud.

**Step 4 — Arabic + polish.** Arabic UI and stories, RTL layout, Arabic font stack, draft banner, `ARABIC-REVIEW.md` export for the native reviewer, final accessibility and motion pass, README with GitHub Pages instructions. Done when: both languages work end to end on phone and tablet, RTL has no layout breaks, every Arabic string is marked draft.

## 9. Arabic policy

- Arabic strings are written as a **draft**, never presented as reviewed.
- A visible notice appears whenever Arabic is active: "مسودة — تحتاج مراجعة من متحدث أصلي" with the English "Arabic draft — pending native review".
- A single constant (`ARABIC_REVIEWED = false`) controls the notice; it only becomes `true` after a native speaker signs off.
- `ARABIC-REVIEW.md` lists every Arabic string with its English source and an id, so a reviewer can mark corrections in one pass.
- No demo to the client with Arabic marked reviewed unless a native speaker really reviewed it. For the internal manager demo, English first; Arabic shown as a labelled draft.

## 10. Out of scope

Accounts, saving or sharing stories, printing, voice narration or text-to-speech, analytics, any AI generation, payments, a real ECA integration, a real events or registration feature.
