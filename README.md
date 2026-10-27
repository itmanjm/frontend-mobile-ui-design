# frontend-mobile-ui-design

A [Hermes Agent](https://hermes-agent.nousresearch.com) skill: the complete quality lane for front-end web and Flutter mobile UI work — visual design, motion, responsive layout, accessibility, and adversarial verification.

## Install

Copy or clone into your skills directory:

```bash
git clone https://github.com/itmanjm/frontend-mobile-ui-design.git \
  ~/.hermes/skills/frontend-mobile-ui-design
```

Or copy `SKILL.md` into any Hermes skills directory: `~/.hermes/skills/<category>/frontend-mobile-ui-design/SKILL.md`.

## What it does

UI work usually fails at one of five quiet gates — generic looks, no motion, broken phones, inaccessible markup, or untested edge data. This skill is a stage-by-stage lane that closes all five:

1. **Visual design** — token-based color systems, typography hierarchy, spacing scales, no dead UI
2. **Motion** — animation that communicates (continuity/feedback/orientation), durations, reduced-motion support
3. **Responsive layout** — web breakpoints + thumb-reach; Flutter's mandatory design-system → responsive → accessibility gate
4. **Accessibility** — contrast, semantics, focus, 200% font reflow, Flutter `Semantics` specifics
5. **Adversarial verification** — hostile-data pass, state pass, handler pass, real-device pass

Plus a lane map (which discipline when: new build, PR review, redesign, native-feel pass), common pitfalls, and a ship checklist.

## License

MIT
