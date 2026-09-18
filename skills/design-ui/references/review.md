# The finish review

Judge the actual experience. A screenshot can establish a visual observation; it cannot establish keyboard access, screen-reader behavior, load speed, or conversion performance.

## Review in order of impact

1. **Identity and task.** Can someone tell what this is and what to do? Does the visual idea belong to the content? Does the page still have character without its logo?
2. **Composition.** Is the dominant subject clear? Do headings and image crops align intentionally? Is white space doing work? Are equal containers being used for unequal content?
3. **Typography.** Check real font loading, line breaks, reading width, leading, numerical alignment, overflow, and hierarchy at every relevant size.
4. **Assets and materials.** Check resolution, crop, focal point, lighting, cutout edges, contact shadows, consistency of icon weight, and fallback media.
5. **Interaction.** Exercise the primary action and its return path. Test menus, view switches, filters, selections, dialogs, forms, and back navigation that the scope includes. Ensure selected state persists where expected.
6. **Responsive composition.** Inspect a narrow phone, an intermediate width, and the primary desktop size. Check landscape or large text where relevant. Avoid using a single phone screenshot as proof of full mobile support.
7. **Access and resilience.** Check keyboard order, visible focus, labels, contrast, reduced motion, and touch alternatives. Then test meaningful empty, error, and loading states.
8. **Motion and performance.** Inspect timing and interruption. Check whether effects delay content, cause layout shifts, or continue rendering unnecessarily. Measure before reporting a performance result.

Fix the three largest visible problems, capture again, and stop repeating checks once the remaining result satisfies the brief. Do not spend the last pass changing tiny shadows while the hero crop is wrong.

## Evidence-based accessibility checks

For ordinary text, WCAG 2.2's minimum contrast criterion uses 4.5:1; qualifying large text uses 3:1. Measure actual foreground and background combinations, including text over changing media. The reference sites were not fully audited for compliance. [W3C contrast guidance](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html).

WCAG 2.2 target-size minimum is 24 × 24 CSS pixels, subject to its defined exceptions and spacing conditions. For new touch interfaces, approximately 44–48px is a useful comfort target, not a claim that every control must have a 44px visible icon. [W3C target-size guidance](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html).

A decorative canvas should not be the only representation of essential links or text. Custom menus need correct semantics, an understandable focus path, and a way to close. A muted caption or hover-only preview observed on a reference is a risk to investigate, not an instruction to repeat it.

## A practical self-critique

Rate these internally as weak, adequate, or strong: hierarchy, specificity to the brand, asset quality, typographic control, interaction clarity, responsive composition, and resilience. Do not average away a failing primary task. The purpose is to identify the next correction, not to invent an objective beauty score.

Typical weak-result diagnoses:

- **Generic despite attractive colors:** no distinctive anchor, ordinary section rhythm, or a hero that could belong to any product.
- **Expensive effects with weak content:** the visual subject is underdeveloped; invest in assets and copy before additional motion.
- **Busy rather than expressive:** too many simultaneous focal points; reduce competing scale, contrast, or movement.
- **Polished desktop, awkward phone:** the composition was scaled rather than redesigned; rewrite layout and crop rules.
- **Beautiful shell, weak app:** key states and real tasks were omitted; complete the task flow.
- **Almost right but visually off:** inspect font metrics, image crop, optical spacing, and alignment before adding more decorations.

## Handoff

Lead with the usable result and how to inspect it. Summarize the design direction and the meaningful checks performed. Name remaining asset, functional, or verification gaps precisely. Keep experimental or untested behavior separate from completed work. Do not claim a universal standard of beauty, accessibility compliance, or performance from the quality of a still image.
