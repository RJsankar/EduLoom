# Homested — AI-Powered Homeschool OS

> **Live Demo → [homested-demo.vercel.app](https://homested-demo.vercel.app)**
> **GitHub → [github.com/RJsankar/EduLoom](https://github.com/RJsankar/EduLoom)**

Homested is a single-file interactive demo prototype for an AI-powered homeschool operating system. It gives parents the curriculum, schedule, and community to teach their kids with total confidence — no setup, no backend, no auth.

---

## What's Inside

| Section | What it does |
|---|---|
| 🏠 Landing Page | Product hero, value props, social proof, CTA |
| 📅 Daily Dashboard | 3-column lesson planner — schedule, viewer, weekly grid |
| 🤖 AI Adjust | Reschedule the day around real-life disruptions |
| 📍 Pod Community | Nearby families, skill-sharing, events, RSVPs |
| 📊 Progress Tracker | Per-child subject bars, AI insight, activity heatmap |
| 🃏 AI Flashcards | Generate flip cards from any topic + audio mode |
| 🤖 AI Tutor | Chat-based homework help + SEL mood journal |
| 🏆 Rewards | Streaks, coins, badges, and a reward store |
| 📁 Portfolio | Report cards, work samples, VR field trip previews |
| 🛍️ Marketplace | Resources, specialist tutors, co-op projects, multilingual packs |

---

## Pre-Loaded Demo Family

**The Hendersons** — no login required, lands directly in the experience.

| Child | Age | Grade | Colour |
|---|---|---|---|
| Emma | 10 | Grade 5 | Indigo |
| Liam | 7 | Grade 2 | Green |

**Emma's lessons today:** Math (Fractions & Decimals) · Science (Solar System) · English (Creative Writing)
**Liam's lessons today:** Reading (Phonics Blends) · Math (Addition to 20) · Art (Finger Painting)

---

## Feature Breakdown

### 📅 Daily Learning Experience

- **Lesson timeline** — vertical schedule with subject colour-coded cards, time labels, and duration badges
- **Lesson states** — Upcoming / In Progress (pulsing dot) / Complete (green ✓)
- **Lesson viewer** — objectives, step-by-step instructions, video placeholder, worksheet download
- **Mark Complete** — confetti burst, status update, next-lesson prompt
- **Quick add** — add a custom lesson inline from the schedule panel
- **Search** — filter lessons by keyword within the schedule

### 🗓 Weekly Overview

- Mon–Fri grid with subject chips colour-coded by type
- Today's column highlighted
- Child switcher syncs with left schedule panel
- **AI Adjust Schedule** — modal flow that simulates intelligent rescheduling

### 🤖 AI Schedule Adjustment

- Free-text input for disruptions (appointments, illness, family events)
- 1.5s simulated AI processing → returns 3 rescheduled time slots
- Apply changes (updates live schedule) or keep original

### 📍 Pod Community

- Family match cards — name, distance, skills they can teach, children's ages
- **Request Connection** — one-click, persists state
- Upcoming events — date, attending count, **RSVP** button (persists state)

### 📊 Progress Tracker

- Per-subject progress bars with colour fill and percentage
- Status labels: `2 lessons ahead 🚀` / `On track ✓` / `1 lesson behind ⚠️`
- Subject filter chips (All / Math / English / Science / History)
- **AI Insight card** — personalised curriculum nudge per child
- **4-week activity heatmap** — lighter/darker squares by day

### 🃏 AI Flashcards

- Topic textarea → **Generate Flashcards** button (1s simulated load)
- 4 interactive flip cards with 3D CSS transform on click
- Pre-loaded Q&A pairs relevant to Emma's current lessons
- **Audio Mode toggle** — simulates text-to-speech readiness
- PDF download placeholder

### 🤖 AI Tutor

- Subject selector pills: Math · Science · English · History
- Chat interface with pre-seeded example exchange
- Send a message → typing indicator → mock AI response
- **SEL Mood Journal** — 5 emoji moods + notes field + save confirmation
- Worksheet generator and read-aloud trigger states

### 🏆 Rewards

- Stats row: streak days · total coins · badge count
- **Badge board** — 6 earned (animated) + 2 locked with unlock conditions
- **Coin store** — Extra Art Time · Choose a Movie · Skip a Worksheet (redeemable, coins decrement)
- **Daily Challenge card** — bonus coin goal tied to lesson completion

### 📁 Portfolio

- Child-specific header and term label
- **Spring Term Report Card** — subject grades, lesson counts, status
- PDF download placeholder
- **Work sample gallery** — 4 recent artifacts with modal preview
- **VR Field Trip previews** — Natural History Museum · NASA Space Center · Ancient Egypt (aligned to active lessons)

### 🛍️ Marketplace

- Search bar + filter pills (All / Videos / Worksheets / Kits / Tutors)
- Resource cards — creator, price, star rating, **Add to Curriculum** (persists state)
- **Specialist tutor booking** — 3 tutors with rate + **Book Session** (persists state)
- **Pod Co-op Projects** — 2 active group research projects with **Join** interaction
- **Multilingual Content Packs** — English · Spanish · Mandarin · Hindi · French switcher

---

## Subject Colour System

| Subject | Colour | Hex |
|---|---|---|
| Math | Indigo | `#4F46E5` |
| Science | Sky Blue | `#0EA5E9` |
| English / Reading | Green | `#10B981` |
| History / Geography | Amber | `#F59E0B` |
| Art / Craft / Music | Pink | `#EC4899` |
| PE / Free | Gray | `#6B7280` |

---

## Layout & Responsiveness

### Desktop (≥ 1024px)
- **Left panel** (280px) — lesson schedule + child switcher
- **Centre panel** (flex) — lesson viewer or welcome card
- **Right panel** (300px) — weekly overview grid + AI Adjust button
- **Below** — scrollable secondary workspace with sidebar nav grouped into Community / Learning / Family

### Tablet (768–1023px)
- 2-column layout, right panel collapses
- Secondary sidebar stacks above content

### Mobile (< 768px)
- Bottom navigation: **Today · Lesson · Community · Progress · Explore**
- Today → full-width lesson timeline
- Lesson → full-screen lesson viewer
- Explore → full secondary workspace (all 8 tabs via group switcher)

---

## Tech Stack

- **Pure HTML** — single file, zero dependencies
- **Embedded CSS** — custom design system, CSS variables, animations
- **Vanilla JavaScript** — state object, event handlers, DOM rendering
- **Google Fonts** — Inter (400, 500, 600, 700)
- **No build tools · No backend · No auth**

---

## Running Locally

```bash
# Option 1 — open directly
open homested.html

# Option 2 — serve locally
npx serve .
# then visit http://localhost:3000/homested.html

# Option 3 — Python
python3 -m http.server 8000
# then visit http://localhost:8000/homested.html
```

---

## Prototype Notes

- All AI flows (schedule adjustment, flashcard generation, tutor replies) are **simulated** — realistic delays and mock responses, no API calls
- PDF downloads, VR previews, worksheet generation, and audio are **placeholder interactions**
- State is held in memory — refreshing resets to the default demo state
- Child names in the visible UI are **Emma** and **Liam** (Henderson family)
