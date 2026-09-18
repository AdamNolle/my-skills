# From art direction to a finished interface

## Composition is the first design system

Create a hierarchy of **anchor → explanation → action → detail**. The anchor may be a face, object, title, useful data, or active task. Its placement and scale should make the first reading obvious.

Use a grid to create relationships, not an obligation to fill equal boxes. Locomotive uses large areas of white space and offset labels; Zhenlong gives neighboring projects different widths; Nick Ho makes the vertical grid visible. Each grid establishes an organizing logic. Repeating three identical cards is appropriate only when the content truly has equal rank.

Specify the page rhythm before styling each section. A useful brand-page sequence might be:

`recognition → proof → explanation → detailed evidence → action`

Vary density deliberately: expansive image, compact text, close-up detail, calm reading space. Keep recurring alignment points so the variation remains coherent. Do not alternate backgrounds simply because the page has another section.

For an app, organize around a primary working area, supporting context, and a stable action region. Let detail appear on selection. Preserve the user's place and selected item when changing a view. Remy's three gallery representations are a useful conceptual reference for continuity, even when the app's implementation is a conventional table and inspector.

## Typography with specific jobs

Choose type by role and available license. The source font name is evidence, not a requirement to acquire it.

| Role | Decisions that matter |
|---|---|
| Display | Character width, contrast, weight, intended line breaks, amount of negative space |
| Reading | Comfortable measure, line-height, punctuation, small-screen legibility |
| Utility | Stable numerals, concise labels, distinguishable states, scannability |
| Signature | A restrained serif switch, custom mark, or expressive word used at meaningful moments |

Observed examples at a 1280 × 720 viewport:

- Remy's sampled paragraph labels declared Roboto Mono, 13.44px, weight 600, 16.128px line-height, and −0.2688px tracking.
- Locomotive's hero declared LocomotiveNew at 70/77px, while navigation declared HelveticaNowDisplay at 26/31.2px.
- Loop's Schoolhouse title declared neoFormaSans at 120/100.8px with −2.4px tracking. Navigation was 16/24px. The broader site also declared neoFormaSerif for selected text.
- Raycast's hero declared Inter at 64/70.4px, weight 600; paragraph text was 18px.
- Forge's sampled section headings declared Geist at 56/58.8px with −1.68px tracking; section reading text used 17/26.35px.

These are component samples, not comprehensive tokens or confirmation that a font file is licensed for reuse. JSON evidence lives beside the screenshots.

When choosing original values, useful starting ranges are display line-height around 0.9–1.1, reading line-height around 1.4–1.65, and body measure roughly 45–75 characters. Adjust for the actual typeface, language, platform, and content. Dense app UI often needs a tighter reading measure and a smaller type scale than a marketing page.

Avoid fixed line breaks that work only at one width. Use a fluid type scale, a maximum text width, and breakpoint-specific composition. Check the narrowest supported screen and enlarged text. Optical alignment may require a small correction where a round letter, quotation mark, or icon makes equal mathematical spacing look unequal.

## Color, surface, and material

Assign colors to roles before choosing decorative shades: canvas, primary text, secondary text, boundary, primary action, selected state, danger, success. Brand accent and status color are separate jobs.

A material has a coherent lighting model. Raycast and EMO use dark fields, restrained highlights, and brighter active areas. Daylight uses environmental warmth and paper-like surfaces. Weird Folders uses surface wear, thickness, occlusion, and directional light. Arbitrary glows on every edge dilute all three approaches.

Choose a radius family based on material and function. Sharp photographic fields can coexist with pill controls. A software panel and its internal button should not automatically have the same radius. Nested corners should feel related rather than inflated.

Use shadows to describe elevation or contact. Product renders need convincing grounding; menus need separation from content. A border often creates enough distinction without a shadow. Distinguish hover, focus, selected, active, and disabled states through more than a subtle shade change.

## Asset specification

For every major image or render, specify:

`subject / purpose / slot ratio / focal point / desktop crop / phone crop / background / light direction / output size / source`

Use deliberate art direction: face orientation should work with nearby text; a product's controls should remain visible; cutout edges and contact shadows should be clean. Keep aspect ratios and intrinsic dimensions stable to avoid jumps while images load.

Prefer real product UI for software demonstrations. Show a recognizable task, a changed state, and a result. A fake dashboard with meaningless graphs undermines the precision-software direction. If a demonstration is visual-only, do not present it as a functioning app.

Use an appropriate image-generation or design tool for missing art. Use an established icon family for utility symbols. Do not counterfeit a complex photograph, product render, or designed icon with text, emoji, placeholder shapes, or improvised code art.

## The app adaptation

Marketing-site taste should improve the interface's quality without slowing the work. Useful transfers include:

- Editorial hierarchy → clear page titles, deliberate column widths, better data grouping.
- Active-image emphasis → distinct selected rows, focused inspectors, legible active tabs.
- Material icon craft → strong app identity and a coherent family of simplified toolbar glyphs.
- Spatial continuity → transitions that preserve the relationship between a row and its detail panel.
- Quiet chrome → fewer competing borders and actions, with labels that remain easy to discover.

For a meaningful product task, design its actual state set: populated, empty, loading, selected, validation error, successful completion, and recovery when applicable. Use representative long titles, missing images, large numbers, and realistic density. Keep destructive actions distinct from primary progress actions. These states are part of the design, not a final engineering cleanup.

Native apps should honor platform conventions for navigation, text scaling, menus, keyboard shortcuts, focus, and input. Express identity within those expectations. A website screenshot is not a native interaction specification.

## Implementation choices

Use the project's existing component and token system first. For a new project, define only the tokens the direction needs. Do not create an elaborate design-system package before one representative screen works.

Build semantic content and working controls before a decorative canvas layer. A gallery can begin as ordinary links and images; a spatial scene can enhance those links. Derive multiple views from one data model so selection, filtering, titles, and counts cannot drift.

Use CSS for simple layout, transitions, and sticky behavior; add a motion library when coordinated timelines or layout transitions justify it. Use WebGL only when depth, lighting, deformation, or spatial navigation creates essential value. These are proposed choices; no original site's stack is asserted by this research.
