# EduLoom Design Lens Audit

Source reviewed: [homested.html](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html)

## Audit Lens

This review looks at the current single-file prototype through:
- UX flow quality
- Nielsen Norman usability principles
- WCAG 2.1 AA accessibility expectations
- mobile-first interaction quality
- emotional tone for both caregivers and learners

## Overall Assessment

The prototype is visually ambitious and has strong product breadth, but it is not yet at "design excellence" or WCAG AA-ready. The main weaknesses are not aesthetic. They are structural:
- several important interactions are not fully accessible by keyboard or screen reader
- some controls are below recommended mobile touch target size
- multiple text treatments likely fail AA contrast on white backgrounds
- the interface still has a few dead-end or placeholder interactions in high-visibility places
- modals are visually present but not implemented as accessible dialogs

The emotional direction is promising:
- rewards and confetti create joy well
- the newer parent-facing surfaces are calmer and more grounded

The next step is to tighten usability and accessibility systems so the experience feels trustworthy, not just polished.

## Critical Issues

### Critical 1: Modals are not accessible dialogs

Why it matters:
- Screen reader users are not told a dialog opened
- keyboard users are not trapped inside the modal
- `Esc` close behavior is incomplete
- close buttons lack explicit accessible naming

Evidence:
- AI modal has no `role="dialog"`, `aria-modal="true"`, or label relationship: [homested.html:3305](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L3305)
- Modal close button has no `aria-label`: [homested.html:3307](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L3307)
- Portfolio preview modal is injected without dialog semantics: [homested.html:5027](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L5027)

Impact:
- blocks WCAG-friendly modal use
- creates a high-friction experience on keyboard and assistive tech

### Critical 2: Keyboard accessibility is incomplete for core flows

Why it matters:
- core flows should not require a mouse or touch
- some interactions are presented like links/buttons but are not semantic controls

Evidence:
- `Keep Original` is an `<a>` with click-only behavior and no `href`: [homested.html:3322](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L3322)
- `Back to Schedule` is also click-only text, not a semantic button: [homested.html:2667](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2667)
- top nav avatar is a clickable-looking `div`, not a button: [homested.html:2507](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2507)

Impact:
- breaks expected keyboard operation
- weakens “user control and freedom”

### Critical 3: Touch target sizes fall below mobile minimums in several important controls

Why it matters:
- WCAG guidance and mobile ergonomics both expect roughly 44x44 px targets
- parents multitasking on mobile need forgiving targets

Evidence:
- nav icon buttons are `36x36`: [homested.html:268](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L268)
- modal close button is `28x28`: [homested.html:1331](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L1331)
- child switcher buttons use very compact padding, and right-panel variants are even smaller via inline style: [homested.html:339](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L339), [homested.html:2677](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2677)

Impact:
- high mis-tap risk on phones
- especially problematic for parents using one-handed navigation

## High Issues

### High 1: Low-contrast text styles likely fail WCAG AA

Why it matters:
- small gray helper text is used heavily across the product
- many labels are 10-12 px with muted gray on white, which is a common AA failure pattern

Evidence:
- global muted text token: [homested.html:22](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L22)
- footer uses `12px` muted text: [homested.html:229](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L229)
- many section labels and helper texts use `11px` muted styling across navigation, cards, and tables: [homested.html:897](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L897), [homested.html:2038](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2038)

Likely failing patterns:
- `#9CA3AF` on white at small sizes
- very small uppercase labels on low-emphasis backgrounds

### High 2: Landing-page flow still contains dead-end navigation

Why it matters:
- the landing page should reduce uncertainty, not create false affordances
- users expect nav links and demo CTAs to lead somewhere real

Evidence:
- top nav links all use `href="#"`: [homested.html:2449](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2449)
- “Watch 2-min demo” shows only a placeholder toast: [homested.html:2463](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2463)

Impact:
- hurts credibility in the highest-visibility funnel
- violates “match between system and real world” and “user control and freedom”

### High 3: ARIA labeling is minimal across icon-only and stateful controls

Why it matters:
- icon-only controls need explicit names
- dynamic controls should communicate state when possible

Evidence:
- notification/settings buttons rely on `title`, not robust accessible labels: [homested.html:2502](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2502)
- no `aria-pressed` or similar state communication on child switchers, subject pills, filter chips, tab buttons, or mood buttons: [homested.html:2519](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2519), [homested.html:2711](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2711), [homested.html:2850](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2850)

### High 4: App hierarchy is strong in some areas but inconsistent across older/newer surfaces

Why it matters:
- newer tabs use context bars and stronger wayfinding
- older surfaces still mix plain headers, emoji-heavy labels, and looser spacing

Evidence:
- newer structured surfaces: [homested.html:2805](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2805), [homested.html:2951](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2951)
- older welcome/lesson areas are flatter and more utility-like: [homested.html:2554](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2554), [homested.html:2621](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2621)

Impact:
- the product feels partially redesigned instead of fully systematized

### High 5: Emotional tone is split between “delightful for children” and “calm for parents,” but not always intentionally

Why it matters:
- rewards and confetti do a good job creating joy
- parents need calmer, lower-cognitive-load areas for planning and review

Evidence:
- reward/confetti delight is strong: [homested.html:812](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L812), [homested.html:4684](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L4684)
- some surfaces still rely heavily on emoji prefixes and playful tone even when task focus should dominate: [homested.html:2788](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L2788), [homested.html:3309](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html#L3309)

Recommendation:
- keep delight strongest in learner motivation surfaces
- reduce decorative intensity in parent planning, compliance, and scheduling surfaces

## Priority Fix Snippets

### 1. Accessible modal semantics and close behavior

```html
<div class="modal-overlay" id="ai-modal" role="dialog" aria-modal="true" aria-labelledby="ai-modal-title" aria-describedby="ai-modal-sub">
  <div class="modal-box" tabindex="-1">
    <button class="modal-close" aria-label="Close schedule adjustment dialog" onclick="closeAiModal()">×</button>
    <div id="modal-input-state">
      <div class="modal-title" id="ai-modal-title">Life got busy? Tell us what changed.</div>
      <div class="modal-sub" id="ai-modal-sub">Describe any changes to your day and we'll reschedule intelligently.</div>
```

```js
document.addEventListener('keydown', (event) => {
  const modal = document.getElementById('ai-modal');
  if (!modal.classList.contains('open')) return;

  if (event.key === 'Escape') {
    closeAiModal();
  }
});
```

### 2. Replace non-semantic click targets with real buttons

```html
<button type="button" class="back-link" onclick="clearLesson()">
  ← Back to Schedule
</button>

<button type="button" class="modal-keep-link" onclick="closeAiModal()">
  Keep Original
</button>
```

### 3. Raise touch targets to mobile-safe minimums

```css
.nav-icon-btn,
.mobile-nav-item,
.child-btn,
.subject-pill,
.filter-chip,
.tab-group-btn,
.modal-close,
.portfolio-view-btn,
.reward-action-btn,
.chat-send-btn {
  min-height: 44px;
}

.nav-icon-btn,
.modal-close {
  min-width: 44px;
}
```

### 4. Improve low-contrast text token usage

```css
:root {
  --text-secondary: #4B5563;
  --text-muted: #6B7280;
}

.nav-group-label,
.portfolio-stat-label,
.section-kicker,
.family-metric-label,
.portfolio-gallery-title,
.modal-sub,
.landing-footer {
  color: var(--text-secondary);
}
```

### 5. Add explicit accessible names to icon/state buttons

```html
<button class="nav-icon-btn" aria-label="Open notifications" title="Notifications">
  🔔
  <span class="nav-badge" aria-hidden="true">2</span>
</button>

<button class="child-btn active" aria-pressed="true" onclick="switchChild('emma')">Anaya</button>
<button class="child-btn" aria-pressed="false" onclick="switchChild('liam')">Lucas</button>
```

### 6. Turn landing nav and demo CTA into real flows

```html
<a href="#value-props">Features</a>
<a href="#secondary-section">Product Tour</a>
<a href="#tab-pod">Community</a>
<button class="btn-secondary" onclick="goToApp()">Open Interactive Demo</button>
```

### 7. Make parent-facing surfaces calmer by design

```css
.parent-hero,
.surface-card,
.portfolio-card,
.modal-box {
  box-shadow: 0 10px 24px rgba(15, 23, 42, 0.05);
}

.parent-hero .hero-kicker,
.section-kicker {
  letter-spacing: 0.04em;
}

.rewards-hero,
.challenge-card-rich {
  /* keep delight here */
}
```

## Figma Recommendations

### Information architecture
- Create one canonical shell component with:
  - top app bar
  - left desktop nav
  - mobile bottom nav
  - page-level context bar
- Split surfaces into two design families:
  - `Family / Parent` surfaces: calmer, denser, lower chroma
  - `Learner / Motivation` surfaces: brighter, more animated, more celebratory

### Accessibility system
- Add a Figma accessibility page with:
  - approved text sizes
  - minimum contrast-approved text tokens
  - minimum 44x44 touch targets
  - modal anatomy with keyboard/focus notes
- Define a component rule that icon-only buttons must always include an accessible-name spec in annotations

### Component cleanup
- Standardize one button system:
  - primary
  - secondary
  - ghost
  - danger or disabled
- Standardize one chip system:
  - filter chip
  - state chip
  - context chip
- Standardize one card header pattern:
  - kicker
  - title
  - helper/status chip
  - supporting copy

### Mobile-first layout
- Start frames at `390px` and `430px` before desktop
- Build bottom-nav and grouped-tab variants as first-class components, not resized desktop leftovers
- Define explicit safe-area behavior for bottom nav and toast stacking

### Emotional design
- Keep confetti and badge motion in learner reward moments
- Remove unnecessary decorative emoji from parent planning and compliance views
- Use softer backgrounds, clearer spacing, and stronger copy clarity for caregiver tasks

## Recommended Next Pass

If the goal is true UX/design excellence, the next highest-value implementation sequence is:
1. fix modal semantics, keyboard flow, and ARIA labeling
2. raise all tap targets to mobile-safe minimums
3. rebalance contrast tokens and small-label styles for AA compliance
4. remove landing-page dead ends and placeholder-feeling primary actions
5. unify older surfaces with the newer page-context pattern

## Bottom Line

The prototype already has a strong product story and noticeably improved visual ambition. What is holding it back is not creativity. It is trust, accessibility, and consistency. Once those are tightened, the design will feel much more production-ready.
