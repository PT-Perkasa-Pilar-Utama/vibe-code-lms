# Product Requirements Document - Usable LMS MVP

> **Document type:** Product Requirements Document<br>
> **Status:** Draft for usable MVP validation<br>
> **Owner:** [TBD]<br>
> **Design owner:** [TBD]<br>
> **Primary audience:** Product, Design, Stakeholders, AI coding agent<br>
> **Phase:** Usable local MVP before production engineering<br>
> **Required artifacts:** Resource findings -> `DESIGN.md` -> `index.html`<br>
> **Last updated:** 2026-08-25

---

## 1. Executive Summary

Create a high-fidelity **usable MVP** for a simple Learning Management System (LMS) that can be opened and used directly before any production application is built. This is not a click-through mockup: every visible core feature in scope must have real client-side behavior, meaningful state transitions, and local persistence where persistence is expected.

The MVP must provide a directly usable learner-facing experience: Landing Page -> Login/Register -> Dashboard -> Course -> Lesson, including working local account/session behavior, progress, completion, resume state, lesson revisitability, and a coherent product brand. The public-facing landing experience must serve as a premium brand showcase with distinctive art direction and cinematic scroll choreography, while authenticated learning surfaces remain calm, legible, and product-first.

The implementation artifact must use exactly one HTML implementation file: `index.html`. It does not need to be dependency-free or fully self-contained. Runtime libraries such as Three.js, GSAP, Lenis, WebGL/shader helpers, or other focused browser libraries may be loaded directly from `index.html` when they materially improve the chosen cinematic direction. Generated media may live under `assets/`. This phase validates both product experience and functional usability. It does not authorize production backend architecture, remote authentication, backend APIs, database servers, or deployment, but it does require the browser-only MVP to behave like a usable product within its local scope.

Core product question:

> **Can a learner understand where they are, what to do next, how they are progressing, and freely revisit material they have already completed?**

---

## 2. Product Context & Problem

Vibe coding can move from idea to implementation very quickly, but an AI agent may fill missing product/design decisions with arbitrary assumptions. This usable-MVP phase exists to make those decisions explicit and executable before production engineering.

The immediate problem is **design, interaction, and usable-flow ambiguity**. The MVP must resolve those decisions with working browser behavior rather than static mock states.

The MVP must avoid:

- arbitrary AI-generated styling,
- inconsistent navigation,
- unclear lesson progression,
- inaccessible or misleading interaction states,
- completed content becoming unavailable,
- scope creep into production engineering.

---

## 3. Desired Outcome

A stakeholder should be able to open the MVP, use it directly, and evaluate the LMS without developer explanation or manual state editing.

A learner should be able to:

- understand the LMS purpose from the landing page,
- enter through Login or Register,
- identify current learning progress,
- open a course,
- understand lesson states,
- open the current lesson,
- revisit completed lessons at any time,
- revisit all learning material after completing a course,
- continue from the latest current lesson without losing progress context,
- complete at least one bundled course end-to-end without external setup,
- close/reopen or refresh the MVP and recover meaningful learner state,
- use browser back/forward or equivalent in-app navigation without breaking the current learner state.

---

## 4. Primary User

### Learner

The MVP focuses on one primary learner role.

The learner needs to:

- discover courses,
- understand course structure,
- consume lessons,
- track progress,
- distinguish completed/current/upcoming states,
- return to previously completed learning material whenever desired.

Do not invent additional personas unless explicitly requested.

---

## 5. Goals

### G1 - Deliver a usable complete MVP learner journey
Landing -> Login/Register -> Dashboard -> Course -> Lesson must be understandable and functionally usable without explanation.

### G2 - Validate information architecture
Navigation, progression, grouping, and actions must be predictable.

### G3 - Establish a reusable Sensio-native visual system
Landing, auth, and LMS product screens must feel like one coherent product, while allowing the Landing Page to use a more expressive premium presentation layer than authenticated learning screens.

### G3A - Create a distinctive premium landing experience
The Landing Page must avoid generic AI/SaaS composition and communicate a deliberate visual point of view through typography, composition, product storytelling, and cinematic scroll choreography. It should feel authored for this LMS rather than assembled from common landing-page templates.

### G4 - Preserve learning access after completion
Completion records progress; it must never remove access to content the learner has already earned or completed.

### G5 - Establish MVP brand identity
The MVP must include a deliberate logo/basic brand identity documented in `DESIGN.md`.

### G6 - Reduce production ambiguity
The approved MVP should become a clear visual, interaction, and behavior reference for a later engineering phase.

### G7 - Make the MVP directly usable
Core learner actions shown in the UI must actually work in-browser. Navigation, local sign-in/register, progress, lesson completion, revisit, resume, and persisted learner state must not be decorative-only interactions.

---

## 6. Non-Goals

This phase does **not** include:

- production frontend architecture,
- framework scaffolding,
- backend APIs,
- database schema,
- production/remote authentication or authorization,
- server-side persistence,
- multi-user synchronization across devices,
- payments,
- certificates,
- forums,
- live classes,
- deployment,
- Docker,
- CI/CD,
- production security hardening.

Mock UI may represent an explicitly out-of-scope concept when necessary, but anything presented as a core in-scope learner action must be implemented as working client-side behavior. Local browser persistence such as `localStorage` or IndexedDB is allowed and expected where needed for usability.

---

## 7. Scope & Priority

Priority uses **Must / Should / Could / Won't (this phase)**.

| ID | Scope item | Priority | User value |
|---|---|---|---|
| S-01 | Landing Page | Must | Understand product value and entry points |
| S-02 | Login | Must | Review sign-in UX |
| S-03 | Register | Must | Review account-creation UX |
| S-04 | Dashboard | Must | See current progress and next action |
| S-05 | Course List | Must | Discover courses |
| S-06 | Course Detail | Must | Understand course structure and progress |
| S-07 | Learning View | Must | Consume lesson content |
| S-08 | Previous/completed lesson revisit | Must | Reopen earlier material |
| S-09 | Completed-course content access | Must | Reopen every completed course lesson after course completion |
| S-10 | Progress and completion states | Must | Understand learning status |
| S-11 | MVP logo / basic brand identity | Must | Give the MVP a deliberate product identity |
| S-12 | Responsive behavior | Should | Keep MVP usable on smaller screens |
| S-13 | Purposeful motion and micro-interactions | Must | Improve clarity and polish |
| S-13A | Cinematic Landing Page scroll experience | Must | Create a memorable premium first impression and product story |
| S-13B | Distinctive non-generic art direction | Must | Prevent template-like AI/SaaS visual output |
| S-14 | Loading/empty/error examples | Could | Demonstrate state design where useful |
| S-15 | Production backend / remote auth / server persistence | Won't | Outside MVP scope |
| S-16 | Local account/session behavior | Must | Login/Register and logout work locally |
| S-17 | Local persistent learner state | Must | Progress, completion, resume, and preferences survive refresh/restart |
| S-18 | Fully functional in-scope controls | Must | Core buttons, navigation, course/lesson actions, and state changes work directly |
| S-19 | Usable navigation/history behavior | Must | Back/forward and screen transitions do not lose or corrupt learner state |
| S-20 | Runtime resilience | Must | Core LMS remains usable if optional cinematic libraries/media fail |

---

## 8. Functional Requirements

### FR-00 - Landing and authentication entry

**Requirement:** The MVP includes Sensio-native Landing, Login, and Register screens with working local account/session behavior.

**Acceptance criteria:**

- Landing Page communicates the LMS value clearly.
- Landing Page has a distinctive premium art direction that is specific to this LMS and does not resemble a generic AI/SaaS template.
- The hero acts as a visual thesis for the product rather than a conventional headline + gradient + floating-card composition.
- The page tells a coherent product story across the full scroll, not a disconnected stack of marketing sections.
- Cinematic scroll choreography is present and materially contributes to orientation, narrative, or product understanding.
- At least one signature visual/interaction moment makes the landing page memorable without compromising usability.
- Login and Register entry points are obvious.
- Login/Register have realistic form structure and working validation/error states.
- Auth screens belong to the same visual system as the product UI.
- Register creates a usable local learner profile in browser storage.
- Login validates against the locally stored learner profile or a clearly documented local demo account.
- Logout ends the local session and returns to the public/auth flow.
- Session state survives refresh when appropriate.
- No remote credentials, OAuth, backend API calls, or server-side authentication are implemented.

### FR-00A - MVP logo and brand identity

**Requirement:** A simple but intentional product logo/basic brand identity must be designed as part of the MVP.

**Acceptance criteria:**

- Logo concept is documented in `DESIGN.md`.
- Logo/wordmark usage is consistent across Landing, Auth, and relevant LMS shell surfaces.
- Branding follows the Sensio-native visual direction rather than introducing a conflicting aesthetic.
- Logo remains practical at compact UI sizes.

### FR-01 - Dashboard orientation

**Requirement:** Learner can immediately identify current progress, active course, and next action.

**Acceptance criteria:**

- Current learning state is visible without opening another page.
- One dominant continuation action is clear.
- Active product navigation is identifiable.

### FR-02 - Course discovery

**Requirement:** Learner can scan available/active courses using meaningful metadata.

**Acceptance criteria:**

- Course title remains visually primary.
- Supporting metadata is secondary.
- Progress is visible where relevant.
- Course items are visually comparable.

### FR-03 - Course structure and lesson revisitability

**Requirement:** Learner can understand course structure, identify what to do next, and reopen any lesson that has already been completed.

**Acceptance criteria:**

- Current, completed, and upcoming/locked lessons are visually distinct.
- **Every completed lesson is interactive and opens its learning content.**
- A completed lesson must never be rendered as disabled solely because it is completed.
- Completed lesson status remains visible while the lesson is revisited.
- Reopening a completed lesson does not reset, reduce, or otherwise alter progress.
- The learner can always return to the latest current learning path after revisiting old material.
- Upcoming/locked state may prevent access only when prerequisite logic explicitly requires it.

### FR-04 - Learning View navigation

**Requirement:** Learner can focus on the current lesson and navigate across all accessible lessons.

**Acceptance criteria:**

- Lesson title/content are the focal point.
- Course position is visible.
- Previous/next controls are understandable.
- Previous navigation works for earlier accessible lessons.
- Clicking a completed lesson from Course Detail opens that exact lesson.
- Revisiting completed material does not mark later lessons incomplete.
- Current learning position remains recoverable after revisiting.

### FR-05 - Progress feedback

**Requirement:** Progress is communicated consistently and independently from content accessibility.

**Acceptance criteria:**

- Completion status records achievement; it does not disable completed content.
- Progress representation is consistent across screens.
- Status is not conveyed by color alone.
- Reopening completed material does not change completion percentage.

### FR-06 - Navigation consistency

**Requirement:** Product and lesson navigation behaves predictably.

**Acceptance criteria:**

- Active location is identifiable.
- Similar actions use similar treatments.
- Back/return behavior is understandable.
- Revisit flows do not strand the learner away from the current learning path.

### FR-07 - Completed course remains fully reviewable

**Requirement:** When a course reaches 100% completion, all lessons in that course remain accessible for review.

**Acceptance criteria:**

- **100% course completion must not lock, disable, hide, or remove lesson content.**
- All lessons in a completed course can be opened directly from Course Detail.
- Previous/next navigation continues to work across accessible lessons.
- Course status may display `Completed`, while individual lesson rows remain interactive.
- Revisiting a completed course does not reset its completed status or progress.
- The UI must not imply that completed learning content is unavailable.

### FR-08 - Local persistence and resume

**Requirement:** Core learner state must persist locally so the MVP can be used across refreshes/restarts without manually reconstructing state.

**Acceptance criteria:**

- learner profile/session state is stored locally;
- course progress and lesson completion persist locally;
- latest current lesson / resume target persists locally;
- revisiting old lessons does not corrupt the persisted current path;
- refreshing or reopening the page restores the expected learner state;
- a clear reset-demo-data action is available for testing/review;
- storage failure falls back gracefully without making the UI unusable.

### FR-09 - In-scope UI must be operational

**Requirement:** Any control presented as a core learner action must perform its advertised action.

**Acceptance criteria:**

- navigation items change to the correct screen/state;
- Login/Register submit flows work locally with useful validation;
- Dashboard continuation opens the correct current lesson/course;
- Course List and Course Detail items open their actual views;
- accessible lesson rows open the correct lesson;
- previous/next lesson controls work where allowed;
- completing a lesson updates progress and the next-current state;
- completed lessons remain revisitable;
- logout works;
- buttons must not be decorative dead controls unless explicitly marked as non-interactive examples.

### FR-10 - Useful demo content

**Requirement:** The MVP includes enough realistic local course/lesson content to be meaningfully usable without external services.

**Acceptance criteria:**

- at least one course can be progressed from start to completion;
- lesson content is substantial enough to test reading/learning flow rather than placeholder lorem ipsum;
- at least one additional course/state may exist to demonstrate discovery and progress variation;
- all demo data is bundled locally with the MVP;
- the user can reset the local demo state without editing source code;
- first use must not require manually editing JSON, browser storage, source code, or developer tools;
- seeded/demo content must be internally consistent so progress can be calculated from actual lesson state rather than hard-coded decorative percentages.

### FR-11 - Navigation and history behavior

**Requirement:** Screen navigation must behave like a usable application even though the MVP is delivered as a single HTML document.

**Acceptance criteria:**

- moving between Landing, Auth, Dashboard, Course, and Lesson views must not require a page rebuild or source edit;
- browser back/forward should restore the expected view when practical, using hash/history state or an equivalent single-document routing strategy;
- refreshing the page must recover a sensible authenticated/public view from persisted state;
- navigation must not accidentally reset course progress, completion, or resume position;
- invalid/stale route state must fall back to a safe usable screen;
- navigation controls shown as available must always lead somewhere meaningful.

### FR-12 - Functional-state integrity

**Requirement:** Displayed learner state must be derived from the same underlying local data used by interactions.

**Acceptance criteria:**

- dashboard progress, course progress, lesson status, completed counts, and resume target are derived from persisted learner/course state;
- completing a lesson updates all affected views consistently without manual refresh;
- revisiting a completed lesson does not create duplicate completion or reduce progress;
- 100% completion is reached from actual lesson completion state rather than a separately hard-coded flag;
- reset-demo-data restores a deterministic baseline across profile/session/progress/resume state;
- no core state exists only as visual text disconnected from application behavior.

### FR-13 - Graceful runtime degradation

**Requirement:** Cinematic presentation dependencies must not be a single point of failure for the usable LMS.

**Acceptance criteria:**

- failure to load Three.js, GSAP, Lenis, shaders, remote fonts, or other optional presentation dependencies must not make Login/Register/Dashboard/Course/Lesson unusable;
- missing generated image/video assets must have intentional poster/fallback treatment without breaking layout;
- if persistent browser storage is unavailable, the MVP should continue in-session where practical and communicate/reset state safely;
- errors in optional landing-page spectacle must degrade to a readable static or reduced-motion presentation;
- no uncaught runtime error may prevent core learner navigation and lesson usage.

### FR-14 - Feature-presence contract

**Requirement:** The final MVP must not advertise functionality that the user cannot actually use.

**Acceptance criteria:**

- if the final DESIGN/index includes search, filter, tabs, profile controls, settings, course CTAs, lesson actions, modal actions, or similar interactive-looking features, they must work within the local MVP scope;
- if a feature is intentionally non-functional or future-only, do not present it as an active control;
- disabled states must have a real product reason, not serve as placeholders for unfinished implementation;
- decorative UI must not masquerade as actionable UI.

---

## 9. Primary User Journey

1. Visitor opens Landing Page.
2. Visitor registers a local learner profile or logs in with an existing local/demo account.
3. Learner enters Dashboard and sees persisted progress/resume state.
4. Learner opens an active course.
5. Learner sees completed/current/upcoming lesson states.
6. Learner opens the current lesson.
7. Learner completes lessons and sees progress update immediately.
8. Learner may revisit any previously completed lesson without losing the current path.
9. Learner refreshes/reopens the MVP and resumes from the persisted state.
10. Learner returns to the current lesson and continues progression.
11. Learner completes the course.
12. Learner can later reopen the completed course and access **all lessons again** for review.
13. Learner can log out, log back in locally, and recover the same stored learner state.
14. Reviewer can reset demo data to a deterministic initial state.

---

## 10. UX & Design Principles - Mandatory

### DP-01 - Product Clarity
Each screen has one clear purpose and dominant focal point.

### DP-02 - Quiet Confidence
Use clean surfaces, restrained color, strong typography, subtle borders, and disciplined whitespace.

### DP-03 - System Over Styling
Typography, spacing, colors, radii, states, and components come from one compact design system.

### DP-04 - UX Before Engineering
Optimize comprehension and task flow before implementation convenience.

### DP-05 - Consistency Beats Novelty
Prefer familiar repeatable patterns over one-off visual tricks.

### DP-06 - Progressive Disclosure
Show information when useful instead of overwhelming every screen.

### DP-07 - State Is Part of the Design
Completed/current/upcoming, hover, focus, selected, disabled, and progress states must be intentionally designed.

### DP-08 - Completion Is Achievement, Not Revocation
A completed state communicates progress. It must **never** visually or functionally suggest that previously available lesson content has become inaccessible.

### DP-09 - Brand Theatre Outside, Product Clarity Inside
The public Landing Page may be expressive, editorial, cinematic, and compositionally bold. Authenticated Dashboard/Course/Lesson surfaces must remain calmer and more task-focused. Premium presentation must not reduce learning clarity.

### DP-10 - Distinctive by Design, Not by Decoration
Avoid the recurring visual defaults of AI-generated landing pages: generic gradient blobs, floating glass cards, meaningless mesh backgrounds, arbitrary 3D shapes, huge centered headline + two CTA formula, fake metric rows, and decorative motion with no narrative purpose. Distinction must come from subject-specific composition, typography, pacing, hierarchy, and one coherent signature idea.

### DP-11 - One Signature System
Choose one memorable landing-page visual motif or interaction system and develop it consistently across sections. Do not combine multiple unrelated trendy effects merely to appear premium.

### DP-12 - Cinematic Experience Over Minimal Motion
The Landing Page must not settle for subtle floating cards, small parallax, repeated fade-ups, or low-intensity decorative motion when the selected reference direction is cinematic. Prefer scene-scale composition, camera-like movement, foreground/background depth, dramatic reveals, spatial transitions, full-viewport moments, and authored pacing where appropriate.

### DP-13 - Reference Grammar, Project Content
The selected reference may come from any subject or industry. Preserve the useful design/motion grammar - composition, framing, camera language, pacing, transition style, 3D/depth treatment, lighting logic, and section rhythm - while replacing the reference subject matter with LMS-appropriate content, imagery, copy, and educational storytelling.

### DP-14 - If It Looks Usable, It Must Be Usable
A learner-facing control must not exist only for visual completeness. If the UI presents an action as available, the action must perform its intended client-side behavior, update shared state correctly, and persist state when persistence is expected. Prefer removing a non-functional feature over shipping a polished dead control.

### DP-15 - One Source of Truth for Learner State
Progress, completion, resume target, current lesson, and dashboard summaries must be derived from one coherent local state model. Do not maintain contradictory decorative counters or duplicated status flags purely for presentation.

### DP-16 - Spectacle Must Fail Gracefully
Landing-page 3D/cinematic effects may be ambitious, but authenticated LMS usability is non-negotiable. Optional visual dependencies or media failures must degrade presentation rather than break the product flow.

---

## 11. Design System Constraints - Sensio Native

Sensio is the mandatory visual reference.

Required product/application direction:

- light, clean, product-first surfaces,
- foreground near `#09090B`,
- Sensio teal `#0D9488` for primary actions/active/progress emphasis,
- white/near-white surfaces,
- subtle `#E4E4E7`-family borders,
- compact professional typography,
- restrained shadow,
- consistent 8-16px radius,
- 8px-based spacing,
- no decorative gradient/glassmorphism/neon treatment inside authenticated learning/product surfaces.

### Landing Page expression layer

The Landing Page may extend the Sensio-native system with stronger editorial typography, dramatic scale contrast, layered composition, controlled depth, full-viewport storytelling moments, and cinematic motion. It must still preserve the core Sensio identity through color discipline, clarity, typography quality, and restrained use of teal.

Landing-specific rules:

- do not default to a generic SaaS hero or centered card stack;
- avoid decorative gradients as a shortcut for visual interest;
- avoid glassmorphism, neon cyber aesthetics, random floating blobs, and unrelated 3D objects;
- product UI, course structure, learning progress, and educational interaction may become visual storytelling material;
- use whitespace and large-scale type intentionally rather than filling every viewport;
- the landing page should have a clear beginning, escalation, and resolution across scroll;
- visual drama must come from composition and motion choreography, not visual noise.

### Lesson Navigation Pattern - Sensio x Dicoding

Use Sensio for visual styling and a Dicoding-like **course-outline navigation model** for lesson behavior.

#### Previous / completed lessons

- Always interactive once they have been completed.
- Use pointer cursor and subtle hover feedback.
- Preserve check/completed status while hovered or opened.
- Hover must be understated: neutral/surface shift, not card lift, glow, or large shadow.
- Keyboard focus must be at least as clear as hover.
- Opening a completed lesson must not alter its completion record.

#### Current lesson

- Strongest state hierarchy in the lesson list.
- Sensio teal may be used as a restrained active indicator/background/border accent.
- Interactive and clearly marked `Current`.
- Current lesson status remains distinct from a previously completed lesson being viewed.

#### Upcoming / locked lessons

- Visually muted where prerequisite gating applies.
- Must not display misleading pointer/hover behavior if not interactive.
- Locked state must have a clear reason/state cue.
- Do not use `disabled` on a lesson merely because it is completed.

#### Completed course

- Course can display a `Completed` status and 100% progress.
- **Every lesson remains interactive.**
- Do not convert all lesson rows into disabled controls after course completion.
- Course Detail becomes a permanent review/index surface for that course's material.

---

## 12. Motion & Animation

Motion is a mandatory part of the premium Landing Page experience and a restrained feedback system inside the LMS product. The two contexts intentionally use different motion intensity.

### 12.1 Landing Page - cinematic scroll choreography

The Landing Page must use motion as narrative structure, not decoration. The scroll experience should feel composed from the first viewport through the final CTA.

Required capabilities/patterns to explore:

- orchestrated hero entrance with clear visual hierarchy;
- scene-scale or full-viewport compositions when supported by the selected reference;
- scroll-triggered section transitions;
- one or more sticky/pinned storytelling sequences where appropriate;
- camera-like push, pull, orbit, pan, dolly, reveal, or perspective changes when they improve the story;
- foreground/background layering with clear depth and occlusion where appropriate;
- coordinated text + product-UI reveals rather than independent random fades;
- WebGL / Three.js / shader-assisted scenes when they materially improve fidelity to the selected reference;
- scroll-scrubbed video, image sequence, generated cinematic media, or hybrid media + DOM choreography when appropriate;
- controlled depth/parallax only where it reinforces spatial continuity;
- product interface transformations or progressive disclosure tied to the learning story;
- intentional section-to-section handoff so the page feels like one continuous sequence;
- cinematic pacing with quiet moments between high-emphasis moments;
- responsive fallback where complex choreography simplifies gracefully on smaller screens.

A cinematic reference must not be reduced into a conventional static landing page with a few subtle animations. The implementation should preserve the reference's perceived scale, pacing, spatial depth, framing, and transition grammar while translating its subject matter into the LMS theme.

The motion must remain scroll-controllable. Do not trap the learner in long unskippable autoplay sequences.

### 12.2 Signature motion moment

`DESIGN.md` must define at least one signature landing interaction that is unique to the chosen visual direction. Examples may include a course-path assembly, a progress journey unfolding through scroll, a lesson interface transitioning from overview to focused learning, or another product-specific transformation.

The signature moment must:

- communicate something meaningful about the LMS;
- be understandable without explanatory text;
- avoid dependence on decorative stock imagery;
- have a reduced-motion alternative;
- remain technically feasible inside the usable-MVP constraints.

### 12.3 Product/application motion

Inside Login, Register, Dashboard, Course Detail, Course List, and Learning View, motion remains restrained and functional:

- state transitions;
- hover/focus feedback;
- progress update;
- auth feedback;
- subtle screen/route transitions;
- lesson-state continuity.

Routine product motion should generally remain under ~300 ms and must never slow repeated learning interactions.

### 12.4 Performance and implementation rules

- Prefer `transform` and `opacity` for normal DOM motion.
- WebGL, Three.js, shaders, canvas rendering, generated video, image sequences, and scroll-scrubbed media are explicitly allowed when justified by the selected cinematic reference.
- External runtime dependencies are allowed if they are loaded directly by the single `index.html`; do not introduce a multi-file application architecture just to use them.
- Prefer focused dependencies over unnecessary framework stacks.
- Avoid layout thrashing and unnecessary simultaneous animations.
- Scroll-driven effects must be tested for smoothness and not assume a high-end GPU.
- Pinned sections must not create broken scroll distance or inaccessible content.
- Animation setup must cleanly adapt when viewport dimensions change.
- If a sophisticated effect cannot remain smooth and readable, simplify the implementation while preserving as much of the cinematic grammar as practical.
- Respect `prefers-reduced-motion`; reduced mode must preserve the full content hierarchy and journey without requiring animation.

### 12.5 Prohibited motion patterns

- unrelated looping ambient motion across the entire page;
- every section using the same fade-up preset;
- random stagger applied to all text/cards;
- excessive cursor-following effects;
- scroll-jacking that overrides expected input behavior;
- long non-interruptible intros;
- motion that exists only to make the page look technically complicated;
- animation that hides content or navigation from keyboard/reduced-motion users.

Detailed timing, sequencing, scroll ranges, trigger behavior, and reduced-motion behavior must be documented in `DESIGN.md`.

---

## 13. Accessibility & Quality Requirements

- Keyboard-operable interactive controls.
- Visible focus.
- No keyboard traps.
- Readable contrast.
- Completion/current/locked status not communicated by color alone.
- Semantic headings and labels.
- Focused controls must not be obscured.
- Completed lesson rows must remain keyboard accessible because they are interactive.
- No unintended horizontal overflow.
- Responsive hierarchy remains usable on smaller viewports.
- MVP remains directly openable/servable and usable without a build step.

---

## 14. Usable MVP Delivery Constraints

The agent must:

1. Produce exactly one implementation HTML file: `index.html`.
2. Keep project-authored CSS and browser JS in that HTML file. Do not split authored runtime code into separate `.css` or `.js` source files.
3. Runtime dependencies are allowed. Three.js, GSAP, Lenis, shader helpers, animation utilities, or other focused browser libraries may be loaded by CDN / ESM / import map / script tag directly from `index.html`.
4. Generated image/video/media assets may live under `assets/` and must use replacement-safe final filenames.
5. Supporting non-runtime artifacts such as `PRD.md`, `DESIGN.md`, asset manifests, prompts, and verification notes may remain separate files.
6. Use local product/course data only; enough realistic data must exist for at least one end-to-end course journey.
7. Persist learner profile/session/progress/completion/resume state in browser storage (`localStorage` or IndexedDB). Do not require a backend for normal MVP use.
8. The MVP must require no application bundler or production build step to run. Opening/serving `index.html` must be sufficient.
9. Every learner-facing control that appears actionable must be wired to real client-side behavior; do not ship decorative dead buttons.
10. Keep one coherent local application-state model for learner/session/course/lesson/progress/resume data; derive UI summaries from that state.
11. Provide deterministic navigation/history behavior appropriate to a single-document app.
12. Provide a deterministic reset-demo-data action so reviewers can return to the initial state without editing source.
13. Ensure optional cinematic dependencies/media degrade gracefully and cannot take down the core LMS flow.
14. Avoid framework scaffolding unless the framework can still satisfy the single-HTML implementation constraint without introducing generated application structure.
15. Avoid backend/database-server/API/remote-auth/deployment implementation.
16. Stop after the usable MVP meets acceptance criteria.

`Single-file HTML` means one authored runtime document, not `zero dependencies` and not `zero media files`. Do not reject a materially better cinematic solution merely because it requires a browser library. Browser-local data/state is part of the usable MVP runtime and is explicitly allowed.

---

## 15. Mandatory Resource Gathering - Before Design

The agent must perform resource gathering **before creating/updating `DESIGN.md`**.

Research at minimum:

1. Sensio visual language.
2. Dicoding-style course/module/lesson navigation.
3. Completed-course review/revisit behavior in learning products.
4. Broad cinematic, motion-heavy, 3D, WebGL, interactive, and award-quality website references across ANY industry or subject - not only LMS/education.
5. At least several candidate references with materially different cinematic/motion approaches before selecting a primary direction.
6. How the selected reference's composition, camera language, pacing, depth, transitions, lighting, and interaction grammar can be translated into LMS-specific subject matter.
7. Appropriate implementation techniques for that direction, including Three.js/WebGL/shaders, GSAP/ScrollTrigger, scroll-scrubbed video/image sequences, DOM choreography, or hybrids when relevant.
8. Login/Register UX.
9. Accessibility for hover/focus/keyboard/status and reduced motion.
10. Responsive LMS/learning layouts.
11. Performance-safe fallbacks for the chosen cinematic treatment.

### Research rules

- Prefer first-party product pages, official design-system documentation, and authoritative accessibility sources.
- Do not copy third-party UI verbatim; extract behavior/patterns.
- Sensio remains the product/brand authority, especially for authenticated LMS surfaces.
- The selected cinematic reference becomes the Landing Page composition/motion authority after adaptation to the project's theme.
- References may come from unrelated industries; their subject matter is not authoritative.
- Dicoding is a behavioral reference for course-outline navigation.
- If research conflicts with this PRD, this PRD wins.
- Do not claim evidence that was not actually inspected.
- Missing evidence becomes `[TBD]`, not invention.

### Required output

`DESIGN.md` must contain **Reference & Resource Findings** covering:

- source/reference,
- observed pattern,
- adopted/rejected decision,
- rationale,
- mapping to PRD IDs.

---

## 16. Mandatory Design Artifact - `DESIGN.md`

Mandatory execution order:

1. Read `PRD.md` fully.
2. Perform resource gathering.
3. Explore relevant skills, including `frontend-design`, `ui-animation`, and GSAP/scroll-animation guidance when available.
4. Create/update `DESIGN.md`.
5. Validate the design gate.
6. Only then create/update `index.html`.

`DESIGN.md` must include:

1. Reference & Resource Findings.
2. Design intent/experience goals.
3. PRD traceability.
4. Information architecture.
5. Screen inventory.
6. Screen-by-screen specifications.
7. MVP logo/brand specification.
8. Design tokens.
9. Component inventory.
10. Component states.
11. **Lesson navigation behavior including completed-course revisitability.**
12. Motion specification, including a landing-page scroll storyboard and signature motion moment.
13. Responsive behavior.
14. Accessibility decisions.
15. Content/local-data guidance, browser persistence model, and reset-state behavior.
16. Visual anti-patterns/non-negotiables.
17. Open design questions `[TBD]`.

### Authority

- `PRD.md` defines scope/requirements.
- `DESIGN.md` defines approved UI/UX expression.
- `index.html` implements `DESIGN.md`.
- Conflict: PRD > DESIGN > implementation.

---

## 17. Acceptance Criteria / Definition of Done

### Product coverage

- [ ] Landing, Login, Register represented.
- [ ] Dashboard, Course List, Course Detail, Learning View represented.
- [ ] Completed lessons are clickable and reopen their material.
- [ ] Completed-course lessons remain fully accessible after 100% completion.
- [ ] Revisiting completed content does not change progress/completion state.
- [ ] Learner can return to the current path after revisiting.
- [ ] Local Register/Login/Logout flow works.
- [ ] Learner session/progress/completion/resume state survives refresh/restart.
- [ ] At least one course can be used from start through completion.
- [ ] Core controls and navigation are operational rather than decorative.
- [ ] Browser navigation/history behavior does not corrupt learner state.
- [ ] Progress/completion/resume values are derived from the same underlying local state.
- [ ] Optional cinematic dependency/media failure does not break core LMS use.
- [ ] No active-looking feature is knowingly shipped as a dead placeholder.
- [ ] Reset-demo-data flow works.
- [ ] MVP logo/basic brand identity exists.

### Design quality

- [ ] Sensio-native product/brand direction is consistent.
- [ ] Landing Page has a premium, distinctive, authored visual direction rather than a generic AI/SaaS template.
- [ ] Landing Page uses a coherent cinematic scroll narrative with at least one signature product-specific motion moment.
- [ ] A cinematic reference is not watered down into minimal card motion, generic fade-ups, or subtle parallax only.
- [ ] The Landing Page preserves recognizable composition, framing, pacing, depth, camera/transition grammar, and scene scale from the selected reference while replacing its subject matter with LMS-specific content.
- [ ] Three.js/WebGL/generated media/scroll-scrubbed media may be used when they improve fidelity to the chosen direction without violating the one-HTML implementation constraint.
- [ ] Landing visual drama does not leak into authenticated learning surfaces in a way that harms clarity.
- [ ] Lesson navigation follows the required Sensio x Dicoding pattern.
- [ ] Completed state is visually positive but never disabled-looking.
- [ ] Current lesson remains visually distinguishable while old material is being reviewed.
- [ ] Focus/hover states communicate interactivity clearly.

### Deliverables

- [ ] Resource gathering completed.
- [ ] `DESIGN.md` reconciled against this PRD.
- [ ] `index.html` implements the current `DESIGN.md`.
- [ ] Usable local learner flows are implemented and browser-persisted.
- [ ] No production backend/framework/deployment scaffolding created.
- [ ] Mandatory Playwright E2E verification passes for every in-scope page/view, core button/control, navigation path, learner-state transition, persistence behavior, responsive/fallback scenario, and the complete Landing Page motion/scroll experience.
- [ ] Landing Page E2E evidence covers every animated scene at multiple scroll-progress checkpoints rather than validating only the initial/final resting state.
- [ ] No animated Landing Page section has unintended overlap, clipping, blank gaps, stale transforms, broken pinning/sticky release, z-index inversion, jump, resize breakage, or content left off-screen after the animation completes.
- [ ] No unresolved console/runtime error, broken route, dead core control, visual-motion defect, or failed mandatory E2E check remains.

---

## 18. Agent Execution Rules

1. Read this PRD fully.
2. Treat requirement IDs as authoritative.
3. Perform mandatory resource gathering.
4. Explore relevant frontend design, UI/UX, design-system, accessibility, responsive, auth UX, motion, micro-interaction, and UX-review skills. When available, explicitly review `frontend-design`, `ui-animation`, `gsap-core`, `gsap-timeline`, `gsap-scrolltrigger`, `gsap-performance`, and framework-appropriate GSAP guidance before defining the landing experience.
5. Record materially relevant findings in `DESIGN.md`.
6. Reconcile `DESIGN.md` before changing HTML.
7. **Never implement completed lessons as disabled/inaccessible content.**
8. **Never lock course content merely because the course has reached 100% completion.**
9. If implementation exposes a missing design decision, update `DESIGN.md` first.
10. Do not modify `index.html` until resource/design gates are satisfied.
11. Review against this PRD and `DESIGN.md`.
12. **Run mandatory full end-to-end verification with Playwright before reporting completion.** Playwright is the required browser-verification gate for this MVP, not an optional smoke test.
13. Before installing Playwright, search the project, workspace, monorepo/root dependencies, global CLI/runtime, package-manager cache, and existing browser-automation setup. Reuse an existing usable installation when found; install only when genuinely unavailable.
14. Exercise the application through the actual rendered UI using visible controls. Verify every in-scope page/view, navigation path, actionable button/control, form flow, interactive state, and core learner journey defined by this PRD.
15. At minimum cover Landing, Register, Login, Logout, Dashboard, Course List, Course Detail, Learning View, previous/next lesson navigation, lesson completion, completed-lesson revisit, completed-course review, resume/current-path recovery, browser back/forward, refresh/reopen persistence, reset-demo-data, responsive behavior, keyboard/focus usability, and degraded runtime/media cases.
16. **Treat the Landing Page as a first-class E2E flow.** A passing test must prove that the complete landing experience remains visually coherent while the user scrolls through it; merely loading the page, reaching the bottom, or checking for JavaScript errors is insufficient.
17. Inventory every landing section and every meaningful motion system before testing: CSS animation/transition, GSAP timeline, ScrollTrigger, sticky/pinned scene, parallax layer, smooth-scroll integration, image sequence, video, canvas, WebGL/Three.js, shader, mask/reveal, fixed overlay, and other scroll/time-driven behavior.
18. For every animated or scroll-driven scene, test multiple deterministic checkpoints: **before entry, early progress (~25%), middle progress (~50%), late progress (~75%), end/release, and after the next section has taken over**. Do not validate only the resting start/end frame.
19. At each checkpoint, collect browser evidence using screenshots plus DOM/computed-style/geometry inspection for the important scene elements. Verify bounding boxes, viewport intersection, opacity/visibility, transforms, dimensions, clipping/overflow, stacking/z-index, and expected fixed/sticky/pinned state.
20. Explicitly fail E2E when any important visible element unintentionally overlaps another, escapes its container, is clipped, leaves an unexplained blank region, appears behind the wrong layer, remains transformed/off-screen after its scene, or changes geometry in a way that breaks the intended composition.
21. For sticky/pinned/ScrollTrigger scenes, verify the full lifecycle: trigger enters at the intended point, pinning remains stable, scrub progress changes continuously, the scene releases at the intended boundary, following content resumes normal document flow, and no spacer/pin residue creates jumps or excessive whitespace.
22. Scroll through the landing page in **both directions**. Reverse scrolling must restore valid scene states without stale classes, stuck transforms, duplicate pinned layers, disappearing content, or broken timeline state.
23. Do not replace the real scroll behavior with direct DOM manipulation for the main E2E pass. Drive wheel/scroll/keyboard behavior through the rendered page and allow the actual smooth-scroll/animation runtime to execute. Programmatic scroll-position setup may be used only as a supplementary deterministic check.
24. Verify motion at more than one desktop viewport and at every project-required responsive breakpoint. Resize/reload where needed and rerun representative motion scenes; a layout that works only at the implementation viewport is not a pass.
25. For canvas/WebGL/Three.js/shader scenes, verify initialization succeeds, canvas dimensions are non-zero and track the container, resize handling works, the scene remains visible during its intended scroll range, and fallback behavior is usable if the runtime or asset fails.
26. For video/image-sequence/time-based media, verify the expected source loads, the visible media occupies the final intended slot, playback/progress changes over time or scroll as designed, loops cross their boundary cleanly where relevant, and poster/fallback/reduced-motion states do not collapse the layout.
27. Detect visual instability during motion, not only after it settles. Sample successive frames/checkpoints and fail on large unintended layout jumps, sudden section-height changes, flash-of-unstyled/blank content, or animation states that temporarily cover essential copy/controls.
28. When a visual baseline or user-provided landing reference is available, capture full-page and scene-level screenshots from Playwright and compare the implementation against the required composition. Significant structural differences, missing scenes, wrong section order, or visibly broken motion states are blockers even if functional assertions pass.
29. During all Playwright runs, inspect console/runtime errors, failed critical network/resource loads, overflow/clipping, unexpected layout shifts, broken focus states, stale UI state, and route/history inconsistencies. Filter only known-benign browser noise with an explicit reason; do not ignore broad error classes.
30. Test at least one true first-use -> register/login -> learn -> complete -> revisit -> refresh/reopen -> resume journey using only visible UI controls.
31. Verify that all visible in-scope buttons and interactive-looking controls either perform their advertised behavior or are intentionally non-interactive according to the PRD; dead core controls are a release blocker.
32. Run the Landing Page motion suite once from a fresh load at the top, once after reload/deep navigation when applicable, and rerun affected scenes after any animation/layout fix. Do not assume one successful run proves deterministic behavior.
33. Any failed mandatory E2E or Landing Motion E2E check is a completion blocker. Fix the implementation and rerun the affected scene/flow plus a broader regression pass before marking the MVP complete.
34. Record concise verification evidence/results in the project documentation or handoff, including tested viewport(s), landing sections/scenes, scroll checkpoints, screenshots or geometry evidence used, media/runtime checks, and any intentionally accepted browser noise. Completion must be backed by real browser evidence, not source inspection alone.
35. **Do not report the Landing Page or MVP complete while motion is visibly messy even if automated assertions are green.** If screenshots or browser observation show broken composition, animation, pinning, layering, or transition timing, the gate has failed and the implementation must be corrected.
36. Stop only after the usable MVP, the general Playwright E2E gate, and the dedicated Landing Motion E2E gate all pass.

---

## 19. Decision Log

| Date | Decision | Reason |
|---|---|---|
| 2026-08-24 | Usable MVP precedes production engineering | Reduce ambiguity/rework while validating real browser behavior |
| 2026-08-24 | Sensio is the visual authority | Keep product language coherent |
| 2026-08-24 | Dicoding-style course-outline behavior is a navigation reference | Keep learning navigation familiar and scan-friendly |
| 2026-08-24 | `DESIGN.md` is mandatory before HTML | Separate design decisions from implementation |
| 2026-08-24 | Resource gathering is mandatory before design | Prevent arbitrary AI assumptions |
| 2026-08-24 | MVP logo/basic branding is required | Make the MVP feel like a deliberate product |
| 2026-08-24 | Completed lessons are always revisitable | Completion should not revoke access to learning material |
| 2026-08-24 | A completed course remains fully reviewable | Learners must retain access to all completed course material |
| 2026-08-24 | Landing Page becomes the premium brand showcase | Create a memorable first impression without sacrificing product clarity |
| 2026-08-24 | Cinematic scroll choreography is mandatory on Landing | Motion should carry the product story rather than decorate isolated sections |
| 2026-08-24 | Authenticated LMS surfaces remain restrained | Separate marketing expression from learning-task clarity |
| 2026-08-24 | Generic AI/SaaS landing conventions are explicitly rejected | Ensure the MVP feels authored and product-specific |
| 2026-08-25 | Cinematic references may come from any industry | Preserve strong motion/design grammar while translating subject matter to the LMS theme |
| 2026-08-25 | Three.js/WebGL and focused runtime dependencies are allowed | Enable high-fidelity cinematic/3D experiences without forcing weak minimal motion |
| 2026-08-25 | Single-file constraint means one authored `index.html`, not zero dependencies/assets | Keep delivery simple while allowing cinematic implementation techniques |
| 2026-08-25 | MVP must be directly usable, not only visually reviewable | Core learner flows should work immediately without backend setup |
| 2026-08-25 | Browser-local account/session/progress persistence is required | Preserve usability while keeping the single-file/no-backend constraint |
| 2026-08-25 | In-scope visible controls must be operational | Prevent a polished MVP from shipping as a collection of dead UI states |
| 2026-08-25 | Learner state uses one coherent browser-local source of truth | Keep progress, completion, resume, and dashboard summaries behaviorally consistent |
| 2026-08-25 | Single-document navigation must behave like a real app | Make the MVP directly usable across view changes, refresh, and browser navigation |
| 2026-08-25 | Cinematic dependencies may degrade, core LMS may not | Keep the usable product functional even when optional spectacle fails |
| 2026-08-25 | Playwright full E2E verification is a mandatory completion gate | Ensure every in-scope page, control, learner flow, state transition, persistence path, and responsive/fallback behavior works in the real rendered application |
| 2026-08-26 | Landing Page motion has a dedicated Playwright E2E gate | Scroll/cinematic experiences must be validated throughout intermediate animation states, reverse scroll, pin/release lifecycle, responsive layouts, media/runtime behavior, and actual visual composition so broken motion cannot pass a shallow smoke test |

---

## 20. Phase Gate - Engineering Must Not Start Yet

Production engineering may begin only after usable-MVP review, critical UX/behavior resolution, approved design direction, and a separate implementation plan/spec.

This PRD does not authorize backend architecture, database-server design, production/remote authentication, backend API implementation, cross-device persistence, deployment, or production security hardening. Browser-local persistence required by this usable MVP is explicitly authorized.
