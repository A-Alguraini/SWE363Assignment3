ASSIGNMENT 3 – ADVANCED PORTFOLIO (README)

Live Demo: https://a-alguraini.github.io/SWE363Assignment3/ (enable Pages on main)
Repository: https://github.com/A-Alguraini/SWE363Assignment3

Overview
A single‑page, fully static portfolio (HTML, CSS, vanilla JS) with three sections—About, Projects, and Contact. This iteration layers on assignment‑3 requirements: visitor state is persisted (name, theme, pinned cards), the Projects view adds multi‑step logic (difficulty filter + tag chips + search + pinned toggle), a GitHub API feed loads lazily, and the UI introduces a session timer plus performance tweaks (content‑visibility, deferred API fetches).

Run Locally
• Open index.html directly in your browser, or
• VS Code → Live Server (recommended to avoid caching of JSON).

Project Structure
index.html
css/
  styles.css
js/
  script.js
assets/
  images/
    gradpic2022.jpg
    kpark.jpg
    classVacancySchedule.png
  projects.json
docs/
  ai-usage-report.md
  technical-documentation.md
technical-documentation.txt
.gitignore

Features (mapped to requirements)
• Advanced Logic & State:
  – Difficulty filter combines with search, tag chips, and title/date sorting.
  – Projects can be pinned, stored in localStorage, and optionally viewed in a “pinned only” mode.
  – Visitor name form persists the preferred greeting; session timer tracks dwell time live.
• API Integration:
  – GitHub feed calls `https://api.github.com/users/A-Alguraini/repos` (top 6 repos).
  – Feed loads only when scrolled into view; manual Refresh button retries if rate‑limited.
  – Graceful states: waiting, loading, success, and failure messaging.
• Data Handling:
  – Local JSON (`assets/projects.json`) is normalized with IDs, tags, and difficulty labels.
  – Loading, Empty, and Error + Retry states remain for the local gallery.
• Animations & Performance:
  – On‑scroll reveal + `prefers-reduced-motion` fallback.
  – `content-visibility`/`contain-intrinsic-size` on cards and feed entries short‑circuit rendering.
  – CSS + GitHub feed assets load lazily (fetch waits for observer intersection).
• UX Feedback:
  – Contact form retains inline validation + toasts.
  – Stats bar surfaces how many projects are filtered, pin counts, and active tag filters.
  – Toast notifications highlight theme/name/pin actions.

How to Customize
• Profile image: replace assets/images/gradpic2022.jpg and make sure the About <img> points to it.
• Projects: edit assets/projects.json. Each project supports:
  title, date (YYYY‑MM‑DD), summary, details, tags [array], image, imageAlt.
• Place all images under assets/images/.

Accessibility
• Semantic landmarks: header, nav, main, footer.
• Keyboard‑operable controls with visible focus outlines.
• role="status" aria‑live="polite" on status regions (Projects + Contact) so screen readers announce updates.
• Descriptive alt text for all images; thumbnails use imageAlt from JSON.

Performance
• CSS preload remains for first paint.
• Project and feed cards use `content-visibility` to skip work offscreen.
• GitHub feed request waits until the user scrolls near it, avoiding unused API calls.
• Thumbnails keep explicit width/height + `loading="lazy"` for CLS control.
• Controls share debounced logic (single render path) to keep scripting lightweight.

Compatibility
Tested manually on: Chrome, Edge, Firefox (desktop), iOS Safari, Android Chrome.

Deployment (GitHub Pages)
1) Repo → Settings → Pages.
2) Source: Deploy from a branch; Branch: main; Folder: / (root); Save.
3) Your site URL: https://a-alguraini.github.io/SWE363Assignment3/
4) Confirm deployment completes, then verify the Live Demo link above.

AI Summary
Documented in `docs/ai-usage-report.md`. Highlights: prompt‑driven brainstorming for the GitHub feed structure, pinning logic, and README outline, followed by manual refactors for accessibility and performance.

License
MIT (or your preferred license).
