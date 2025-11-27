TECHNICAL DOCUMENTATION

Architecture
- HTML: Single index.html with three sections (About, Projects, Contact). Projects render from a <template id="projectItemTemplate">.
- CSS: css/styles.css defines design tokens (colors), layout (grid/cards), focus styles, chips, accordion, toast, and reveal transitions.
- JavaScript: js/script.js manages state, data fetch/Retry, rendering, animations, toasts, and contact-form validation.

Architecture
- HTML: Single index.html with three sections (About, Projects, Contact). Projects render from a `<template id="projectItemTemplate">` and the GitHub feed uses `<template id="repoTemplate">`.
- CSS: css/styles.css defines design tokens, layout, focus styles, chips, accordion, toast, reveal transitions, stats bar, timers, and feed cards.
- JavaScript: js/script.js handles state, normalization, GitHub API integration, rendering, form validation, timers, and notifications.
  tags: new Set(),
  activeTags: new Set(),
  sort: 'title',   // 'title' | 'date'
  query: ''
};

Rendering & Interaction
1) Init: set year, apply theme, set greeting, init tabs, reveal observer, and form handler.
  query: '',
  difficulty: 'any',
  onlyPinned: false,
  pinned: new Set(load('pinnedProjects', [])),
  github: { repos: [], loaded: false, loading: false, error: '' }
3) Filters: buildTags() collects unique tags; applyFilters() combines query + active tag chips + sort.

Helper utilities:
- `slugify` + `normalizeProjects` ensure every project has an id/difficulty and preserves extra properties.
- `persistPins` writes the pinned set back to localStorage.
4) Cards: renderProjects() clones the template per item, fills title/date/summary/details, renders tags, and wires the accordion. If image exists, it shows .thumb with loading="lazy" and imageAlt.

Accessibility
- Landmarks: header / nav / main / footer.
3) Filters: buildTags() collects unique tags; applyFilters() combines query + active tag chips + difficulty select + optional pinned-only mode + sort (title/date) while keeping pinned items ahead of the rest.
4) Cards: renderProjects() clones the template per item, fills title/date/summary/details/difficulty, renders tags, wires the accordion, and handles the pin button (aria-pressed + localStorage persistence). Thumbnails still lazy-load with explicit sizes.
5) Stats: updateStats() surfaces total vs. visible projects, pinned count, active tag filters, and difficulty selection.
6) GitHub feed: IntersectionObserver triggers fetchGitHubRepos() once the feed enters the viewport (lazy). A Refresh button retries manually. The API call hits `https://api.github.com/users/A-Alguraini/repos?sort=updated&per_page=6`, maps the response, and renders repo cards with stars, language, and relative updated time.
7) Visitor form: nameForm saves or clears the preferred greeting in localStorage. Session timer shows elapsed time since load.
- All images include descriptive alt; thumbnails use imageAlt from JSON.

Error, Loading, and Empty States
- Loading: “Loading projects…” while fetching.
- Error: network failure message with a Retry button.
- Empty: “No projects found.” when filters match nothing.
- All status messages are announced via aria-live.

Animations
- On-scroll reveal uses IntersectionObserver with threshold: 0.12. When an element becomes visible, .visible is added and the observer unobserves it to avoid jank.
- Reduced-motion users skip animations via the prefers-reduced-motion media query.

Performance
- CSS preload to improve first paint:
  <link rel="preload" href="css/styles.css" as="style">
  <link rel="stylesheet" href="css/styles.css">
- Thumbnails have explicit width/height and loading="lazy" to reduce CLS.
- Project/feed cards use content-visibility + contain-intrinsic-size to reserve space before rendering.
- GitHub fetch waits for viewport visibility (lazy data loading) and uses AbortController for timeouts.
- Minimal JS/CSS; no third-party libraries.

Compatibility
Chrome (Desktop): OK — Baseline dev browser
Edge (Desktop):   OK
Firefox (Desktop): OK
iOS Safari:       OK — Tap targets >= 44px
Android Chrome:   OK — Lazy images work as expected

Data Format (assets/projects.json)
{
  "projects": [
    {
      "title": "K Park Parking App",
      "date": "2025-10-10",
      "summary": "Branding and UI concept for a campus parking app.",
      "details": "Designed logo treatment and explored flows for finding available parking.",
      "tags": ["ui", "branding", "campus"],
      "image": "assets/images/kpark.jpg",
      "imageAlt": "K Park app logo"
    },
    {
      "title": "Class Vacancy Schedule",
      "date": "2025-10-12",
      "summary": "Editable weekly schedule for open class slots.",
      "details": "Printable table and a clean HTML/CSS version for sharing.",
      "tags": ["planning", "html", "table"],
      "image": "assets/images/classVacancySchedule.png",
      "imageAlt": "Class vacancy schedule table"
    },
    {
      "title": "Personal Portfolio",
      "date": "2025-10-15",
      "summary": "Single-page portfolio with greeting, theme toggle, and contact form.",
      "details": "Reveal animations, search/sort/tag filters, and validated form.",
      "tags": ["web", "portfolio", "javascript"],
      "image": "assets/images/gradpic2022.jpg",
      "imageAlt": "Author portrait"
    }
  ]
}

Future Work
- Persist filters in the URL for shareable views.
- Pagination (“Load more”).
- Real email delivery using Formspree / EmailJS / a serverless endpoint.
- Offline cache for projects GitHub feed (IndexedDB) if rate-limited.
