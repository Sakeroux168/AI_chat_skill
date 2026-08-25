---
name: gui-acceptance
description: Decide when human visual acceptance is required; automated screenshots never substitute for it.
---

# GUI Acceptance

## Requires human visual acceptance

A change touching any of these must be explicitly flagged for **真人视觉验收**
in the delivery report and PR:

- UI layout, spacing, alignment
- Fonts / typography / glyph rendering
- Colors, themes
- Images, video output, animation
- Drag & drop, interaction feel (手感)
- Any claim of "visual fidelity"

The flag is a statement that the machine did NOT verify the visual result —
not a formality.

## Does not require human GUI acceptance

Pure backend work: schema changes, CLI tools, headless services, data
pipelines, internal refactors with no observable UI change. Standard code
review and tests are sufficient.

## Hard rules

1. **Automated screenshots do not impersonate human judgment.** An agent may
   capture a screenshot as *evidence for a human to review*; it may never
   write "visually verified".
2. Borderline case (e.g. logic change inside a UI component): judge by whether
   rendered output can differ visibly — if yes, flag it.
3. The reviewer's report must carry a "Human / Visual Verification Required"
   section whenever the diff touches anything visual.
