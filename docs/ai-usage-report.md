

# AI Usage Report

## Tools Used
- ChatGPT: brainstorming the GitHub feed layout, pinning strategy, difficulty filter UX, and README outline.
- VS Code IntelliSense: in-editor completions, quick fixes, and CSS property hints.

## Representative Prompts
1. “Design a GitHub repos widget that loads on scroll, includes refresh, and summarizes stars/language/updated date.”
2. “How can I add pin/unpin buttons to cards and persist them in localStorage while keeping the UI accessible?”
3. “Give me sentence starters for documenting AI usage and performance optimizations in a student README.”

## Raw Outputs (Short Excerpts)
- Skeleton fetch logic with AbortController for GitHub.
- Idea for using aria-pressed buttons to represent pinned cards.
- Outline bullets for AI usage + performance documentation.

## Your Edits & Rationale
- **API Integration**: rewrote the suggested fetch block to normalize repo data, limit to six entries, and add lazy loading via IntersectionObserver.
- **State Management**: extended the idea of aria-pressed buttons into a full pinning workflow (Set + persistence + stats).
- **Performance**: merged AI hints with `content-visibility`/`contain-intrinsic-size` tweaks, added reduced-motion handling, and deferred network calls until necessary.
- **Docs**: rephrased AI output to match personal tone and included concrete metrics (which filters are active, etc.).

## Challenges
- Respecting GitHub’s anonymous rate limits while ensuring the UI never blocks; solution: lazy fetch + refresh button + friendly states.
- Keeping multiple filters (search, tags, difficulty, pinned) in sync without redundant renders; solved with a single applyFilters pipeline.
- Preventing layout shift for dynamically loaded cards; addressed with contain-intrinsic-size and reserved heights.

## Learning Outcomes
- Managing richer app state (query, sort, difficulty, pinned set, GitHub status) in vanilla JS.
- Chaining multiple observers (reveal + feed) without hurting performance.
- Documenting AI usage with enough specificity to show understanding and edits.

## Innovation
- Pin-first workflow with live stats and persistence.
- GitHub feed that feels native to the portfolio (lazy loading, refresh, aria-live messaging).
- Visitor personalization via stored greeting + live session timer.
