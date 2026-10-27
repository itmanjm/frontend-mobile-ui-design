---
name: frontend-mobile-ui-design
description: Use when building, reviewing, or redesigning any front-end web UI or Flutter mobile UI. Enforces the full quality lane — visual design, motion, responsive layout, accessibility, and adversarial verification — so shipped UI is distinctive, native-feeling, and unbreakable.
version: 1.0.0
author: Jozu Works
license: MIT
metadata:
  hermes:
    tags: [frontend, mobile, flutter, ui-design, accessibility, animation, design-system]
    related_skills: [frontend-design, mobile-design-system]
---

# Front-End & Mobile UI Design — the Full Quality Lane

## Overview

Most UI work fails not at "does it render" but at one of five quieter gates: it looks generic, it doesn't move, it breaks on real phones, it's inaccessible, or it was never tested with hostile data. This skill is the complete lane: it tells you which discipline to apply at each stage of a web or Flutter build, the concrete rules for each, and the verification pass that must run before anything ships.

The lane has five stages, always in this order:

1. **Visual design** — make it intentional, not template-shaped
2. **Motion** — make it feel alive, then restrain it
3. **Responsive layout** — web: adaptive breakpoints; Flutter: the mandatory gate trio
4. **Accessibility** — non-negotiable, checked before build, not after
5. **Adversarial verification** — try to break the UI before a human sees it

## When to Use

- Building a landing page, marketing site, web app, dashboard, or portfolio
- Building or reviewing any Flutter screen, widget tree, or mobile flow
- Redesigning an existing UI (web or mobile)
- Reviewing a PR that touches UI code
- Making a web app feel native on a phone (PWA polish)

**Don't use for:** pure backend work, CLI tools, or data pipelines (skip to code-review skills), or brand/logo creation (graphic-design territory, not UI).

---

## Stage 1 — Visual Design

**Rule: every screen ships with a point of view.** If a stranger could guess it was AI-generated from the layout alone, it failed.

- **Start from real references, not imagination.** Study proven product flows (screenshot libraries like Mobbin are the fast-follower's toolkit). Lift the skeleton that works — screen order, stage count, where the primary action sits — then differentiate on brand, not structure.
- **Typography does the heavy lifting.** One display face, one body face, max. Establish hierarchy with size/weight/spacing before reaching for color. Line-height: 1.5+ body, 1.1–1.2 display.
- **Color is a system, not a palette.** Define semantic tokens (surface, surface-variant, primary, accent, success, danger, muted-text) and use only those. Brand colors: navy/blue + one warm accent beats seven hues. Never put accent color on large surfaces.
- **Spacing on a scale.** Pick a 4px (or 8px) base scale and never freehand a margin. Adjacent related items get *less* space than unrelated groups — grouping-by-space is the cheapest clarity tool that exists.
- **No dead UI.** Every visible control works, every state renders (loading, empty, error, success). A button that does nothing is worse than no button.
- **Dark mode is two designs,** not one design with inverted colors. Check elevation shadows, image treatment, and accent contrast separately.

## Stage 2 — Motion & Animation

**Rule: motion communicates; decoration distracts.** Every animation must answer "what does this tell the user?"

- **The three legitimate jobs of motion:** (1) *continuity* — an element that expands from where it was tapped; (2) *feedback* — press states, confirmations, undo toasts; (3) *orientation* — showing where new content came from.
- **Durations:** 150–250ms for micro-interactions, 300–500ms for transitions. Anything over 500ms had better be a deliberate hero moment. Ease-out for entrances, ease-in for exits.
- **One moving thing at a time.** Concurrent animations on unrelated elements read as noise. Stagger lists 20–40ms per item, max.
- **Respect the system:** honor `prefers-reduced-motion` (CSS) / `MediaQuery.disableAnimations` (Flutter) — collapse to opacity fades or nothing.
- **Audit existing code like a senior:** hunt for animations that fight scroll, block input, run off-screen, or animate layout properties (`width`/`top`) instead of compositor-friendly ones (`transform`/`opacity`).
- **Find the gaps before adding:** list every user action with no visible response — those are the animation opportunities that matter more than adding flair to things that already respond.

## Stage 3 — Responsive & Mobile Layout

### Web
- Design from content out, not viewport in. Breakpoints at content-breaking points, not device names. Typical ladder: 480 / 768 / 1024 / 1440.
- **Thumb-reach matters on mobile web:** primary actions in the bottom third. Hit targets ≥ 44×44px. No hover-only affordances (touch has no hover).
- The native-feel checklist for web-on-phone: no scroll jumps on input focus, `-webkit-fill-available` height handling, momentum scroll containers, input `font-size ≥ 16px` (kills iOS zoom), overscroll behavior contained, safe-area insets respected.

### Flutter — the mandatory gate trio
Load **all three** before writing or reviewing any Flutter UI, in this order:

1. **Design system first.** Components, spacing tokens, and theme come from the design system — never ad-hoc `TextStyle(fontSize: 14)` inline. One source of truth for color/typography/spacing.
2. **Responsive layout second.** Use `LayoutBuilder`/`MediaQuery` deliberately: constraints-based layouts that survive 320px–tablets. Test the smallest supported device AND the largest. Safe areas (`SafeArea`) on everything that touches screen edges.
3. **Accessibility third.** Contrast ≥ 4.5:1 for text, touch targets ≥ 48×48dp, `Semantics` labels on every interactive element, no information conveyed by color alone. Run the audit **before** the build, not as a post-launch cleanup.

**State management rule:** when combining Riverpod with auth/GoRouter, guard provider initialization order — assertion crashes from reading a provider before it exists are the classic silent killer. (See flutter-riverpod-gorouter-auth patterns.)

## Stage 4 — Accessibility (both platforms)

- Contrast: ≥ 4.5:1 body text, ≥ 3:1 large text/UI components. Verify computed colors, not source colors (theme layers can override).
- Every interactive element reachable and operable by keyboard (web) / screen reader (both).
- Focus states: visible, never `outline: none` without a replacement.
- Form fields: real `<label>`s / `labelText`, error text tied to the field, `aria-live` for async errors.
- Text scales: test at 200% system font size — layouts must reflow, not clip.
- Flutter specifics: `Semantics` widget on custom gestures, `MergeSemantics` for composite rows, `ExcludeSemantics` on decorative visuals.

## Stage 5 — Adversarial Verification (before any ship)

**Try to break it on purpose.** The UI that survives the ugly-data pass is the one that survives users.

- **Hostile data pass:** 500-char strings, emoji, RTL text, `0`, `-1`, `NaN`, empty lists, single-item lists, 10,000-item lists, missing images, currencies, names with hyphens/apostrophes/CJK. Every text surface gets the longest realistic value.
- **State pass:** loading (skeleton or spinner — never a frozen screen), empty (with a next action, not a blank void), error (recoverable, human-readable), offline, slow network, double-tap, back-press mid-animation.
- **Handler pass:** overlapping actions verified individually — each button/trigger fires its own handler exactly once; no double-submit, no dead zones.
- **Device pass (mobile):** smallest + largest supported screens, dark mode, 200% font scale, interrupted app-resume, keyboard-open state.
- **Post-deploy pass (web):** broken-link audit, console error sweep, computed-style spot-check (CSS layer resets silently beating utilities is a known silent killer — verify *computed* styles).

**Completion rule:** a UI task is done when a human has seen it working on a real target device/environment — not when the code compiles.

---

## Common Pitfalls

1. **Skipping straight to code.** One approved reference/mockup before building prevents three rebuilds. Gate features behind approved design, always.
2. **Designing for the happy path only.** Empty/error/loading states are design work; budget review time for them.
3. **Animals-in-the-headlight motion.** Adding animations because the framework makes it easy. If it doesn't communicate, delete it.
4. **Accessibility as a v2.** Retrofitting semantics and contrast costs 5× doing it inline. It's Stage 4 of the same build, not a future ticket.
5. **Trusting the emulator/screenshot.** Real-device pass for mobile, real-browser pass for web. Simulators hide safe-area, keyboard, and performance issues.
6. **Inline styles in Flutter.** They bypass the design system and rot instantly. Route every visual constant through tokens/theme.
7. **Testing with clean data.** `Test User` and `lorem ipsum` hide every real-world break. The hostile-data pass exists because users type things you didn't imagine.
8. **Vague completion claims.** "Looks good on my machine" is not verification. Evidence = screenshot/recording on target device + exercised states.

## Verification Checklist

- [ ] Visual: token-based colors/spacing only; hierarchy reads in 3 seconds; zero dead UI
- [ ] Motion: every animation communicates; reduced-motion honored; nothing over 500ms without reason
- [ ] Responsive: smallest + largest targets tested; web thumb-zone respected; Flutter SafeArea everywhere
- [ ] Accessibility: contrast verified (computed), touch targets sized, semantics labels present, keyboard/screen-reader path works, 200% font reflow tested
- [ ] Adversarial: hostile-data pass, state pass (loading/empty/error/offline), handler pass, real-device pass
- [ ] Evidence: screenshots/recording on target device attached; human sign-off before ship

## Lane Map (which discipline when)

| You are… | Apply |
|---|---|
| Starting a web build | Stage 1 → 2 (find motion gaps first) → 3 → 4 → 5 |
| Starting a Flutter build | Gate trio (design system → responsive → a11y) → 2 → 5 |
| Reviewing a UI PR | Stage 4 checklist + hostile-data pass minimum; motion review if animations touched |
| Redesigning existing UI | Stage 1 audit → adversarial pass on current UI to collect real failures → rebuild with the lane |
| Making web feel native on phone | Stage 3 web checklist + Stage 2 feedback motion |
