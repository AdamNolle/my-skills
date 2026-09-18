---
name: design-ui
description: Design, build, or refine distinctive websites and app interfaces using Adam's visual taste, grounded in 22 studied references. Use for art direction, page composition, typography, motion, authentic product media, polished product UI, logos, favicons, and icon direction; also when asked for a beautiful, premium, or reference-inspired interface. Preserve an existing product's design system and the current brief. Do not apply to unrelated backend work or treat every interface as an immersive portfolio.
---

# Design UI

Make interfaces with a clear point of view, exceptional composition, and convincing materials. Beauty must survive real content, small screens, keyboard navigation, and repeated use.

## The user's taste

The user supplied 22 references and especially emphasized **Remy Shoots, Studio Loop, and Locomotive**. They singled out **Parakeet and Weird Folders for icons**. Give those preferences extra weight when the current brief leaves room for interpretation. This is a taste library, not a request to copy another identity or impose a single house style.

The strongest common traits are:

- An authored visual idea: a photographic instrument, a moving collage, an editorial publication, a physical object, or a spatial collection.
- A dominant subject and deliberate scale contrast. Small, quiet navigation makes large images or type feel larger.
- Typography as composition: controlled line breaks, asymmetry, optical alignment, and purposeful contrast between display and utility text.
- Asset quality that carries the design. Photography, physical renders, and carefully drawn icons do more work than ornamental containers.
- Motion that changes relationships, reveals structure, or focuses attention.
- Repeated details that belong to the same visual world: rules, labels, ticks, corners, materials, crop behavior, and active states.

Do not reduce this taste to dark backgrounds, oversized headlines, gradients, or rounded cards. Several references use these; none of those treatments alone creates their quality.

## Prefer authentic product evidence

For a site or campaign about a specific physical product, use authentic, licensed product photography or footage as the default visual evidence when suitable media exists. Preserve the real object's proportions, finish, controls, wear, logos, and period details. Background extraction, crop, color correction, cleanup, grading, and restrained compositing may improve presentation; do not regenerate the product merely to make the source more convenient.

Use original 3D work when it reveals something photography cannot: an exploded assembly, transparent shell, material transition, cutaway, motion study, or unavailable viewpoint. Keep technical claims proportional to the references used and label interpretive models when internal geometry is simplified. Generated imagery is appropriate for original campaign art and genuinely missing visuals, not as an automatic substitute for a real product.

Read [product-media-and-brand.md](references/product-media-and-brand.md) whenever the work involves physical products, product video, background removal, 3D/Blender assets, a logo, favicon, app icon, or site mark.

## Choose the work, then the direction

1. Establish the audience, primary action, content, platform, existing system, and available assets from the request and project. Ask only for missing information that would materially change the design; otherwise state a reasonable assumption and continue.
2. Distinguish a **brand encounter** from a **repeated task**. A cinematic entrance can suit a campaign. A dashboard needs fast orientation, information density, stable controls, and clear state changes.
3. Read [design-directions.md](references/design-directions.md). Choose one dominant direction and, if useful, one supporting influence with a different job. Explain the choice in a short sentence. Do not blend all the references.
4. Read the relevant entries in [site-studies.md](references/site-studies.md) and inspect their linked screenshots. Read the three emphasized references when designing an expressive portfolio or studio site. Read the icon reference when the task concerns app icons or distinctive pictograms.
5. For close matching, inspect the current source at the needed viewport and state. The bundled captures document **2026-09-17**, not every version of these sites. Separate observed behavior, measured CSS, and proposed implementation. Never infer a framework, shader, timing curve, or performance score from appearance.

## Write a small design contract

Before substantial implementation, define these decisions in working notes or the project's existing design document. Keep this proportional to the job; a small edit does not need a new specification.

- **Purpose:** what the visitor should understand or accomplish first.
- **Art direction:** one sentence with subject, atmosphere, composition, and signature behavior.
- **Reference roles:** which reference supplies layout, which supplies motion or material, and what is deliberately original.
- **Typography:** display, reading, and utility roles; widths, scale relationships, line-height, and real font availability.
- **Composition:** page grid, dominant anchor, image crops, section sequence, and density changes.
- **Surface system:** palette roles, borders, radii, shadows, and icon family. Use existing tokens when working inside a product.
- **Asset plan:** real source, licensed asset, user-provided asset, or generated asset for each critical slot. Name missing assets early.
- **Brand artifacts:** official or original logo/mark strategy, favicon and app-icon exports, smallest-size behavior, and light/dark variants.
- **Signature interaction:** trigger, response, state continuity, touch alternative, and reduced-motion version.
- **Small-screen composition:** what stacks, crops, disappears, or becomes a different interaction.

When exploration is requested, offer genuinely different compositions rather than palette variations. When a direction is already selected, implement it without an unnecessary approval checkpoint.

## Build from the largest decisions down

Read [craft-and-build.md](references/craft-and-build.md) when implementing or refining a UI.

Work in this order: content and dominant asset → composition and type → responsive structure → interaction and states → motion → optical detail. Fix a weak image crop or generic layout before polishing a shadow.

Establish one representative screen or section at high fidelity, including a narrow layout, before propagating its rules. Use real content lengths and representative data. Preserve the current stack unless the task gives a reason to change it.

For apps, apply the user's taste through proportion, icon craft, typography, surface depth, and transitions between meaningful states. Keep navigation and primary tasks familiar. Do not turn operational screens into a sequence of dramatic marketing scenes.

For motion-heavy work, read [motion-and-responsive.md](references/motion-and-responsive.md). For icons, read [icons.md](references/icons.md). These are implementation approaches inspired by the research, not claims about the original sites' code.

## Review the result as a designed object

Read [review.md](references/review.md) before handoff. Inspect actual rendered states, not only source code.

- Compare desktop and phone screenshots with the design contract. If matching a reference, compare the same viewport and state side by side.
- First fix hierarchy, spacing, image crop, type metrics, and density. Then fix borders, radii, shadows, and motion details.
- Exercise the primary task and its meaningful states. Check navigation, keyboard focus, touch behavior, long content, reduced motion, and missing media where applicable.
- Report what was built, what was actually checked, and any material gap. A polished screenshot is not proof that the app works or meets accessibility or performance targets.

## Evidence and originality

[site-studies.md](references/site-studies.md) contains the complete 22-site research, numbered capture steps, evidence scope, and transfer lessons. [The screenshot index](evidence/index.json) maps the accepted captures to their sources. Exact CSS samples are available for selected references; other numeric recipes in this skill are proposed starting points.

Use the underlying relationships, not someone else's logos, product photography, copy, or distinctive brand assets. The bundled screenshots are research evidence, not production assets. Obtain or create appropriate assets for the user's project. Do not promise an award-winning outcome; build and verify the craft that makes a strong result possible.
