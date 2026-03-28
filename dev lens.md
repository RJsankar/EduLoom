# EduLoom Dev Lens Audit

Source reviewed: [homested.html](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html)

## Scope

This audit evaluates the single-file HTML/CSS/JS prototype for production readiness across:
- performance
- cross-browser and device resilience
- runtime robustness
- code quality and maintainability

## Executive Summary

The prototype is impressive for a single-file demo, but it is not production-ready yet.

The main blockers are:
- too much inline, monolithic code in one `233 KB` HTML file
- no persistent state layer despite stateful UX expectations
- many string-based `innerHTML` render paths and inline handlers
- missing defensive handling for empty or malformed data
- several browser-risky visual effects and layout assumptions

Current file size:
- `233024` bytes
- `5246` lines

## Lighthouse Prediction

This is an estimate based on source inspection only. I did not run Lighthouse in a real browser in this environment.

### Predicted scores
- Performance: `70-82`
- Accessibility: `68-78`
- Best Practices: `65-75`
- SEO: `75-85`

### FCP prediction
- Likely `1.4s - 2.3s` on desktop-class hardware
- Likely worse on low-end mobile due to:
  - large inline CSS and JS in one document
  - Google Fonts dependency
  - heavy paint/compositing from blur, shadows, gradients, and multiple animations

### Why it may miss ideal production targets
- entire app shell, data, styles, and behavior ship in one blocking document
- no code splitting
- no asset lazy loading strategy
- many animated surfaces use `box-shadow`, `backdrop-filter`, and transform-heavy hover effects

## Critical Bugs / Risks

### Critical 1: Stateful interactions do not persist across refresh

Category:
- robustness
- product correctness

What happens:
- pod requests, RSVPs, lesson notes, completed lessons, marketplace actions, redeemed rewards, and language selections are all in-memory only
- a refresh loses user actions

Evidence:
- no `localStorage` or `sessionStorage` usage anywhere in file
- state lives only in `state` object and mutated DOM

Relevant code:
- state definition around [homested.html:3540](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L3540)
- action handlers around [homested.html:4467](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L4467), [homested.html:4478](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L4478), [homested.html:5004](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L5004), [homested.html:5041](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L5041)

Repro:
1. Open app
2. RSVP to an event or connect to a pod
3. Refresh page
4. State is lost

Why it matters:
- breaks trust
- makes multi-step workflows non-credible

Suggested fix:

```js
const STORAGE_KEY = 'eduloom-prototype-state-v1';

function serializeState() {
  return {
    activeChild: state.activeChild,
    activeTab: state.activeTab,
    activeTabGroup: state.activeTabGroup,
    completedLessons: [...state.completedLessons],
    rsvpdEvents: [...state.rsvpdEvents],
    connectedFamilies: [...state.connectedFamilies],
    lessonNotes: state.lessonNotes,
    stepsDone: Object.fromEntries(
      Object.entries(state.stepsDone).map(([k, set]) => [k, [...set]])
    )
  };
}

function saveState() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(serializeState()));
}

function loadState() {
  const raw = localStorage.getItem(STORAGE_KEY);
  if (!raw) return;
  const parsed = JSON.parse(raw);
  state.activeChild = parsed.activeChild || state.activeChild;
  state.activeTab = parsed.activeTab || state.activeTab;
  state.activeTabGroup = parsed.activeTabGroup || state.activeTabGroup;
  state.completedLessons = new Set(parsed.completedLessons || []);
  state.rsvpdEvents = new Set(parsed.rsvpdEvents || []);
  state.connectedFamilies = new Set(parsed.connectedFamilies || []);
  state.lessonNotes = parsed.lessonNotes || {};
  state.stepsDone = Object.fromEntries(
    Object.entries(parsed.stepsDone || {}).map(([k, arr]) => [k, new Set(arr)])
  );
}
```

### Critical 2: Empty-data and missing-data paths are not safe

Category:
- robustness

What happens:
- multiple render functions assume data arrays always exist and always contain at least one item

Evidence:
- direct indexing and sorting on `progress`, `lessons`, and family summaries
- `strongest` / `support` assume non-empty progress arrays
- parent summary assumes aggregated averages exist

Relevant code:
- [homested.html:4528](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L4528)
- [homested.html:4133](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L4133)

Repro:
1. Remove all lessons for one child in data
2. Open schedule or progress
3. Some sections render incorrectly or risk undefined access

Why it matters:
- brittle in real production data conditions
- makes QA coverage harder

Suggested fix:

```js
function safeArray(value) {
  return Array.isArray(value) ? value : [];
}

function getStrongest(progress) {
  const items = safeArray(progress);
  if (!items.length) return null;
  return items.slice().sort((a, b) => b.pct - a.pct)[0];
}

function renderEmptyState(title, body) {
  return `
    <div class="surface-card">
      <div class="section-heading">${title}</div>
      <div class="section-copy">${body}</div>
    </div>
  `;
}
```

### Critical 3: Monolithic single-file architecture is too coupled for production maintenance

Category:
- code quality
- maintainability

What happens:
- data, styles, markup, state, and all behaviors are tightly coupled in one file
- one change can easily regress an unrelated area

Evidence:
- `5246` lines in one document
- rendering functions and data definitions all share global scope
- heavy dependence on global IDs and manual DOM replacement

Impact:
- hard to test
- hard to diff safely
- hard to modularize future features

Recommendation:
- split into at least:
  - `index.html`
  - `styles.css`
  - `data.js`
  - `app.js`
  - `views/` or section render modules

## High Bugs / Risks

### High 1: Performance risk from large inline bundle

Category:
- perf

Evidence:
- entire CSS and JS are embedded directly in HTML
- file size is `233 KB` before compression

Impact:
- larger HTML parse and execute cost
- slower FCP on mobile
- no browser caching separation between markup, CSS, and JS

Repro:
1. Simulate slow 4G + mid-tier Android
2. Load page cold
3. Expect parse/paint delay from single bundled document

Suggested fix:
- split CSS and JS into external files
- preload only critical font assets
- inline only truly critical CSS for landing hero if needed

### High 2: Visual effects are expensive on lower-end mobile and Safari

Category:
- perf
- cross-browser

Evidence:
- repeated `backdrop-filter`
- many large shadows
- multiple infinite animations
- hover transforms on many cards
- animated shimmer/bounce/floating badge effects

Relevant code:
- `backdrop-filter` around [homested.html:218](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L218), [homested.html:1081](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L1081), [homested.html:1311](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L1311)
- rewards animations around [homested.html:1908](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L1908)

Why it matters:
- Safari and lower-end mobile GPUs often struggle with blur + layered shadow + opacity animation combinations

Suggested fix:

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation: none !important;
    transition: none !important;
    scroll-behavior: auto !important;
  }
}

.tab-content-wrap,
.value-card,
.modal-overlay {
  backdrop-filter: none;
}

@supports (backdrop-filter: blur(10px)) {
  .tab-content-wrap,
  .value-card,
  .modal-overlay {
    backdrop-filter: blur(10px);
  }
}
```

### High 3: Resize/orientation handling is incomplete

Category:
- cross-browser/device

Evidence:
- only a simple `resize` handler exists
- no orientation-specific recovery logic
- no state reconciliation when switching from mobile section mode back to full layout beyond a minimal reset

Relevant code:
- [homested.html:5222](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L5222)

Repro:
1. Open on mobile width
2. Go to `pod` or `progress`
3. Rotate device or resize across breakpoint boundaries
4. Some panels may return in a partially-reset state

Suggested fix:
- centralize layout mode computation
- re-run a single `syncLayoutMode()` on load, resize, and orientation changes

### High 4: Inline event handlers reduce testability and maintainability

Category:
- code quality

Evidence:
- extensive use of `onclick`, `oninput`, `onkeydown`

Impact:
- harder to unit test
- harder to lint and refactor
- mixes behavior into markup strings

Suggested fix:
- replace inline handlers with delegated listeners

```js
document.addEventListener('click', (event) => {
  const button = event.target.closest('[data-action]');
  if (!button) return;

  const action = button.dataset.action;
  if (action === 'open-ai-modal') openAiModal();
});
```

### High 5: Repeated `innerHTML` rendering increases bug surface and future injection risk

Category:
- robustness
- code quality

Evidence:
- many sections fully replace large DOM blocks with `innerHTML`
- user-provided content is currently limited, but future expansion could create sanitization issues

Relevant code:
- schedule rendering
- tutor messages
- portfolio modal
- tab panels

Why it matters:
- easy to break focus state, event continuity, and selection state
- harder to incrementally update UI

Suggested fix:
- use template helpers with element creation for dynamic/user-provided content
- keep `textContent` for freeform strings

### High 6: Missing defensive error handling

Category:
- robustness

Evidence:
- no `try/catch` around storage, parsing, or DOM assumptions
- no guarded bootstrap if required elements are missing

Impact:
- one malformed DOM or future data issue can fail an entire feature silently

Suggested fix:

```js
function byId(id) {
  const el = document.getElementById(id);
  if (!el) throw new Error(`Missing required element: ${id}`);
  return el;
}

function safeRun(label, fn) {
  try {
    fn();
  } catch (error) {
    console.error(`[${label}]`, error);
  }
}
```

## Medium Risks

### Medium 1: Timer and animation intervals may do unnecessary work

Evidence:
- timer uses `setInterval`
- multiple `setTimeout` and `requestAnimationFrame` combinations

Risk:
- acceptable for a prototype, but should be cleaned up if multiple timers or long sessions are expected

### Medium 2: Hardcoded copy, dates, and labels reduce adaptability

Evidence:
- hardcoded greeting date and several stats
- mixed placeholder text embedded directly in render functions

Why it matters:
- localization, theming, and CMS/data-driven productionization become expensive

### Medium 3: Placeholder actions are mixed with real stateful actions

Evidence:
- several buttons display toasts instead of real feature behavior

Risk:
- acceptable in a prototype
- but in production this blurs what is implemented versus mocked

## Cross-Browser / Device Audit Notes

### Likely safe
- modern flex/grid layout usage
- no framework dependency
- mostly simple JS APIs

### Needs explicit testing
- Safari rendering of `backdrop-filter`
- iOS fixed bottom nav + safe-area behavior
- resize/orientation transitions across `767px`
- hover-heavy affordances on touch devices
- gradient and blur stacking performance on older iPhones

### Browser/device risk summary
- Chrome desktop: low to medium risk
- Edge desktop: low to medium risk
- Safari desktop: medium risk
- iOS Safari: medium to high risk
- low-end Android Chrome: medium to high perf risk

## Bundle / Architecture Recommendations

### Recommended first refactor
1. Extract `styles.css`
2. Extract `data.js`
3. Extract `app.js`
4. Add a small storage module
5. Group renderers by section:
   - `renderSchedule`
   - `renderParentDashboard`
   - `renderProgressTab`
   - `renderRewardsTab`
   - `renderPodTab`

### Recommended data normalization

```js
const learners = {
  anaya: { ... },
  lucas: { ... }
};

const appData = {
  learners,
  pods: [...],
  events: [...]
};
```

### Recommended constants cleanup
- move strings like section labels, toast messages, and placeholder copy into config objects
- move demo-only values like dates and sample counts into one seeded data source

## Predicted Production Readiness Score

If `100` means “safe to begin production implementation with minimal architectural risk,” current readiness is roughly:

- `52/100`

### Breakdown
- Product demo quality: `85/100`
- Frontend maintainability: `45/100`
- Performance safety: `60/100`
- Cross-browser confidence: `55/100`
- Data/state robustness: `35/100`

## Recommended Next Fix Order

1. add `localStorage` persistence and restore flows
2. add empty-state and null-safe rendering guards
3. replace blocking `alert()` with inline/toast validation
4. reduce blur/shadow/animation cost and add reduced-motion support
5. split file into modular CSS/JS/data units
6. replace inline handlers with delegated listeners
7. add real browser-based Lighthouse and Safari/iOS validation

## Bottom Line

EduLoom is a strong prototype, but not yet production-ready in its current single-file form. The biggest engineering gaps are persistence, defensive rendering, maintainability, and mobile/browser performance confidence. Once those are addressed, the project will be much safer to evolve.
