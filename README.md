# EduLoom

EduLoom is a single-file frontend prototype for an AI-powered homeschool operating system. It combines a marketing landing page with an interactive family learning dashboard designed for planning, instruction, community coordination, motivation, and portfolio tracking.

## Product Snapshot

### Current household setup
- Family label: `Rao-Miller family`
- Learners: `Anaya` and `Lucas`
- Experience type: frontend-only prototype with mocked data and simulated AI flows

### Core experience
- Landing page for product positioning and early-access messaging
- Daily learning workspace with lesson schedule and lesson viewer
- Family-level parent dashboard for aggregated insights and quick actions
- Child-specific learning, rewards, tutor, portfolio, and community surfaces
- Responsive UI that supports both desktop and mobile layouts

## Navigation and Layout

### Landing page
- Branded hero with CTA buttons and social-proof messaging
- Visual product preview cards
- Value proposition sections for curriculum, flexibility, and community
- Gradient-heavy visual treatment with hover states and responsive behavior

### App shell
- Sticky app header with date, streak indicator, avatar, and notification badge
- Three-panel desktop learning workspace
- Dedicated mobile bottom navigation for core lesson flow
- Grouped secondary navigation organized into:
  - `Community`
  - `Learning`
  - `Family`
- Dedicated tabs for:
  - `Parent Dashboard`
  - `Pod Community`
  - `Progress Tracker`
  - `AI Flashcards`
  - `AI Tutor`
  - `Rewards`
  - `Portfolio`
  - `Marketplace`

### Learner switching
- Shared child switchers across schedule, weekly planning, and progress surfaces
- Child-aware rendering for:
  - lesson schedule
  - weekly grid
  - progress tracker
  - rewards dashboard
  - portfolio header/content
  - AI tutor context

## Family-Level Features

### Parent dashboard
- Family-level completion summary across both learners
- Combined streak and planned learning hours
- Family readiness score for documentation and exam alignment
- Aggregated subject averages across children
- Weekly trend sparklines for each learner
- AI recommendation cards driven by simple threshold logic
- Compliance/export preview cards
- Quick actions for:
  - AI schedule adjustment
  - portfolio preview
  - pod review
  - marketplace access
  - tutor launch

### AI schedule adjustment
- Modal-based “AI Adjust Schedule” workflow
- Input area for appointments, fatigue, or family changes
- Simulated AI-generated rescheduling suggestions
- Option to apply the adjusted plan or keep the original schedule
- Modal close support via button, overlay click, and `Esc`

## Daily Learning Experience

### Daily schedule
- Left-side lesson timeline for the active learner
- Subject color coding across lessons
- Lesson states for upcoming, in-progress, and completed work
- Contextual lesson actions such as `Start`, `Resume`, and `Review`
- Completed lesson styling and selected lesson highlighting

### Lesson viewer
- Detailed lesson page populated from in-file data
- Child name, grade, subject label, and progress state
- Materials list and learning objectives
- Step-by-step instructional flow
- Optional video and worksheet blocks
- Notes area and lesson completion state

### Completion flow
- “Mark complete” interaction for active lesson
- Live completed lesson counter
- Confetti celebration animation
- Automatic next-lesson prompt
- End-of-day completion state when all lessons are done

### Weekly overview
- Weekly learning grid for the active learner
- Subject-based chips and color coding
- Current-day visual emphasis

## Community and Support Surfaces

### Pod Community
- Redesigned learner-aware pod dashboard
- Family match cards with skills, children, and distance
- Request connection interactions with persistent state
- Upcoming event cards with RSVP actions
- Co-op and local-learning discovery patterns

### AI Flashcards
- Topic input area for generating quick study prompts
- Four interactive flip cards with 3D flip behavior
- Audio mode toggle
- Placeholder PDF export action for flashcard sets

### AI Tutor
- Mock chat interface with a pre-seeded example exchange
- Subject selector for `Math`, `Science`, `English`, and `History`
- Simulated AI replies with typing state
- Subject-aware conversation seeding instead of always showing the same math example
- SEL mood journal with emoji check-in and notes area
- Utility actions like worksheet generation and read-aloud trigger states

### Marketplace
- Featured curriculum resource cards
- “Add to Curriculum” interactions
- Specialist tutor booking cards
- Pod co-op project cards with join interactions
- Multilingual content pack switcher with feedback states

## Learner Progress and Motivation

### Progress Tracker
- Redesigned child-specific progress dashboard
- Horizontal subject cards with momentum bars and status pills
- Subject filters
- AI insight card with personalized curriculum suggestion
- Compact 4-week activity heatmap
- Learner-specific rendering so the dashboard updates correctly when switching children

### Rewards
- Learner-specific rewards dashboard
- Summary tiles for streak, coins, and badge count
- Animated earned badge cards with stagger, bounce, and shimmer effects
- Softer locked badge states for clearer hierarchy
- Reward store with redeem interactions
- Daily challenge card

## Portfolio and Documentation

### Portfolio
- Child-specific learning portfolio header
- Spring term report card section with grades, lesson counts, and statuses
- Placeholder PDF download action for report exports
- Redesigned work-sample gallery with stronger card hierarchy
- Modal-based sample preview interactions
- VR field trip preview cards for:
  - Museum
  - NASA
  - Egypt

### Compliance and export readiness
- Parent dashboard attendance summary
- Portfolio preview access from family dashboard
- Prototype readiness scoring for documentation and certification flows
- Placeholder export actions for PDFs and report artifacts

## UI and Interaction Improvements

### Visual redesign work completed
- Notification badge clipping fix in the top nav
- Grouped secondary navigation replacing the crowded tab row
- Improved portfolio section hierarchy and showcase presentation
- Redesigned progress tracker layout
- Redesigned reward store into better horizontal cards
- Animated rewards badge board
- Updated parent dashboard as a dedicated family surface

### Responsive behavior
- Desktop-first multi-panel layout
- Mobile bottom navigation for core lesson flow
- Mobile-safe secondary tab visibility
- Wrapped and grouped tab behavior for smaller screens
- Responsive card/grid adjustments across major sections

## Data Modeled in the Prototype

- Two learners with separate schedules and progress models
- Daily lesson schedules and lesson metadata
- Weekly subject plans
- Per-child progress percentages and AI insight text
- Family-level aggregate analytics and recommendations
- Rewards, coins, badges, and streak data
- Portfolio report card and work sample content
- Pod families and upcoming community events
- Flashcard prompts and answers
- Tutor sample conversations and SEL states
- Marketplace resources, tutors, co-ops, and multilingual packs

## Tech Stack

- Plain HTML
- Embedded CSS
- Vanilla JavaScript
- No build tools
- No backend dependencies

## Project File

- [homested.html](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html): landing page, app UI, styling, data models, and interaction logic

## How to Run

Open [homested.html](/Users/ramyasankar/Documents/RJ Hackathon/Homeschooling/homested.html) directly in a browser.

You can also serve the folder locally:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000/homested.html`.

## Prototype Notes

- This is a frontend prototype with mocked content and simulated AI behavior
- PDF downloads, tutor responses, schedule adjustment, worksheet generation, and several export flows use placeholder interactions
- Internal data keys still use legacy child IDs under the hood, while the visible UI shows the updated names `Anaya` and `Lucas`
