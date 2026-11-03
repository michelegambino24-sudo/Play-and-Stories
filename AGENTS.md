# AGENTS.md — rules for Codex in this repo

Project: **Play & Stories**, a static concept-demo website. Read `SPEC.md` first. It is the source of truth. If SPEC.md and a request disagree, say so and ask before building.

## How a session works

1. **One step per session.** Work only on the step the user names from SPEC.md section 8. Do not start the next step, and do not add features that are not in the step.
2. **Plan first, then wait.** Start with a short plan (files you will create or change, key decisions, anything unclear, any new file or font you want to add). Then **stop and wait for the user's OK**. Do not write code before the OK.
3. **Build, test, stop.** After the OK: build the step, run `node tests/check-data.js` when it exists, do a manual check against the list in SPEC.md section 7, then stop.
4. **Final message:** what was built, which files changed, the manual checklist for the user to try, and anything left open. Keep it short.
5. **Never commit, push, tag or create branches.** The user commits himself. Do not change git configuration.

## Language

- Chat replies, code, comments, file names and commit suggestions are in **English**.
- Product text is English first. Arabic only appears in Step 4, unless a step says otherwise.

## Design skills (mandatory for any UI work)

Use these before and while designing or changing visuals, layout, type or motion: **hallmark, impeccable, emil-design-eng, review-animations, better-typography, transitions-dev**.

- At the start of the plan, say which of these skills are available in this folder.
- If one is not installed, **say so and stop**; do not skip it silently and do not install it yourself.
- Run review-animations on every animation or transition you add. Run better-typography on every text block.

## Hard constraints

- Plain HTML, CSS, JavaScript only. No frameworks, no build step, no bundlers, no runtime npm dependencies, no `package.json` needed for the site.
- Relative paths only (the site also runs from `/<repo-name>/` on GitHub Pages and by double-click). Classic `<script>` tags, no ES modules, no `fetch()` of local files.
- **No network at all from the pages:** no CDN, no Google Fonts, no embeds, no analytics, no tracking. A font is allowed only if the file is self-hosted, open-licence, committed with its licence, and approved in the plan.
- **No AI calls, no API keys, no paid services, no accounts.** Do not buy, sign up for, or log in to anything.
- **Do not use Chrome, any browser automation, or computer use.** Check your work by reading code, running Node scripts, and listing what the user should check by hand.
- No real children's names or data anywhere, including examples. No data stored except the language choice (localStorage).
- **Images:** original SVG only, written by you. No stock images, no copying or tracing existing characters, logos or artwork. Never use the ECA logo, name, text or illustrations. The screenshots in `docs/reference/` are inspiration only.
- **Concept badge** ("Concept demo — not an official product") must be visible on every page.
- **Arabic** is draft until reviewed: show the draft notice whenever Arabic is active and never claim it is reviewed. Do not translate in a way that hides that it needs review.
- Do not read or edit `.git/`, `node_modules/`, or `docs/reference/` image files beyond viewing them for design reference.

## Quality bar

- Big tap targets (at least 56 px; 72 px+ for children's picture cards), very little text, works at 360 px wide and on tablets.
- Real `<button>` and `<a>` elements, visible focus, correct `lang` and `dir`, aria-live for results.
- Every text/background pair passes WCAG AA. Remember that white on the coral background does not.
- Respect `prefers-reduced-motion` in every animation. No flashing, no autoplay sound.
- Keep each page light and the code readable: small files, clear names, short comments where the logic is not obvious.
- Keep content libraries in `js/data/` as plain data, separate from logic.
- Test scripts have zero dependencies and never touch the network.

## Do not

- Do not add features, pages or libraries that are not in SPEC.md.
- Do not rewrite or reformat files you were not asked to change.
- Do not leave placeholder text such as "Lorem ipsum" or "TODO" in anything the user will show.
- Do not describe unfinished work as done. If something could not be completed or checked, say it plainly.
