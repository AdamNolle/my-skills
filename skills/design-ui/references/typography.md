# House typography: Instrument Serif + Archivo

The user's preferred pairing for new work is **Instrument Serif** for display and signature moments, and **Archivo** for reading, interface, and utility text. Use it by default when a project has no established type system. When a product already has a type system, keep that system unless the brief asks for a rebrand.

Facts below were verified on 2026-10-13 against the Google Fonts metadata and the font binaries in the Google Fonts repository. Guides are the [Complete Guide to Instrument Serif](https://www.beautifulwebtype.com/instrument-serif/) and the [Complete Guide to Archivo](https://www.beautifulwebtype.com/archivo/) on Beautiful Web Type.

## The two faces

| | Instrument Serif | Archivo |
|---|---|---|
| Designers | Rodrigo Fuenzalida, Jordan Egstad (for the Instrument brand) | Omnibus-Type |
| Character | Condensed display serif: tall, narrow, sharp, high-contrast | Grotesque sans based on late-19th-century American type; low contrast, large x-height |
| Styles | **400 Regular and 400 Italic only**, with no other weights | Variable: `wght` 100–900, `wdth` 62–125 (extra-condensed to expanded), italic on both axes |
| Useful OpenType | `liga`, `dlig` (e.g. fb, fh, fk, qj), `ss01` (alternate a, c and others), `ordn`, `case` | `tnum`, `lnum`/`onum`, `pnum`, `zero`, `frac`, `sups`/`subs`, `ordn`, `case` |
| Metrics (units/em 1000) | cap 720, x-height 510 | cap 686, x-height 526 |
| License | SIL OFL 1.1 | SIL OFL 1.1 |
| Sources | Google Fonts; [Instrument/instrument-serif](https://github.com/Instrument/instrument-serif) | Google Fonts; [Omnibus-Type/Archivo](https://github.com/Omnibus-Type/Archivo) |

Archivo Black and Archivo Narrow exist as separate families. The variable Archivo's `wdth` axis already covers narrow and expanded widths, so prefer it to loading those families.

## Roles

| Role | Face | Starting point |
|---|---|---|
| Display / hero | Instrument Serif 400 | Large sizes (≈56–160px desktop), line-height 0.9–1.0, tracking 0 to −0.01em. Its narrowness allows bigger type and more words per line than a regular-width serif. |
| Signature emphasis | Instrument Serif Italic | One word or phrase inside an Archivo or serif line. Use it where the meaning changes, not as a decoration on every heading. |
| Section headings | Instrument Serif at ≥ 32px, **or** Archivo 600–700 at `wdth` 100–112 | Choose one rule per project and keep it. |
| Reading / body | Archivo 400, `wdth` 100 | 16–19px, line-height 1.45–1.6, 45–75 character measure. |
| Interface | Archivo 450–550, `wdth` 100 | Buttons, inputs, navigation, menus. Medium weight reads well at small sizes. |
| Utility / labels / metadata | Archivo 500–600, `wdth` 75–87, uppercase with +0.04–0.08em tracking, or sentence case | Captions, indexes, specs, tags. The condensed width creates a tool-like voice without adding a monospace face. |
| Data and numerals | Archivo with `font-variant-numeric: tabular-nums` (`slashed-zero` where codes appear) | Tables, prices, timers, version numbers. |
| Impact / poster | Archivo 800–900 at `wdth` 112–125 | Use sparingly; it competes with the serif. One loud moment per page. |

The contrast that gives the pairing character is **narrow, elegant serif against a sturdy, adjustable grotesque**. Make that contrast visible: large serif with small, carefully tracked Archivo labels; never two medium-sized lines of equal weight in both faces.

## Rules

- **Never fake-bold Instrument Serif.** It has only one weight. Set `font-synthesis: none` (or `font-synthesis-weight: none`) on serif elements. Do not use it for UI controls, dense tables, or text that needs a bold state; create hierarchy through size, italic, or a switch to Archivo.
- **Keep Instrument Serif out of small reading text.** Its contrast and narrow forms suit about 24px and larger. A short serif pull quote around 20–24px is acceptable; paragraphs belong to Archivo.
- **Mix the faces within a line deliberately.** Their x-heights are close (510 vs 526), so an Instrument Serif italic word inside an Archivo sentence at the same `font-size` sits well. If it looks small at your size, apply `font-size: 1.04em` to the serif span, adjusting by eye.
- **Use width as a hierarchy tool.** Archivo's `wdth` axis is the system's secret weapon: condensed for metadata, normal for reading, expanded for an occasional loud word. Keep to 2–3 named width stops per project rather than arbitrary values.
- **Balance display wraps.** Use `text-wrap: balance` on serif headlines and `text-wrap: pretty` on paragraphs. Check line breaks at every breakpoint; condensed type makes widows and very long lines easy to miss.
- **Use the features.** Turn on `tabular-nums` for changing numbers, `case` for all-caps labels so punctuation centers, and `ss01` on Instrument Serif only after checking the alternate a/c in context. Discretionary ligatures (`dlig`) are for display moments only.
- **Avoid uniform letter-spacing.** Large serif: 0 to slightly negative. Archivo body: 0. Small uppercase Archivo: positive.

## Loading on the web

Prefer self-hosting the variable WOFF2 files in production (both licenses allow it). The Google Fonts API is fine for prototypes:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Archivo:ital,wdth,wght@0,62..125,100..900;1,62..125,100..900&family=Instrument+Serif:ital@0;1&display=swap" rel="stylesheet">
```

Load only what the design uses. If italics or the full width range are unnecessary, narrow the request (e.g. `Archivo:wdth,wght@75..112,400..700`). Preload the one or two files that render above the fold.

In Next.js, use `next/font/google`. Pass `axes: ['wdth']` for Archivo and `weight: '400', style: ['normal', 'italic']` for Instrument Serif.

```css
:root {
  --font-display: "Instrument Serif", "Iowan Old Style", "Times New Roman", serif;
  --font-sans: "Archivo", ui-sans-serif, system-ui, -apple-system, "Helvetica Neue", Arial, sans-serif;
}

.display {
  font-family: var(--font-display);
  font-weight: 400;
  font-synthesis: none;
  line-height: 0.95;
  text-wrap: balance;
}

.label {
  font-family: var(--font-sans);
  font-weight: 560;
  font-stretch: 80%;            /* maps to wdth 80 */
  text-transform: uppercase;
  letter-spacing: 0.06em;
  font-feature-settings: "case";
}

.data { font-variant-numeric: tabular-nums slashed-zero; }
```

`font-stretch` percentages map directly to Archivo's `wdth` axis. Use `font-variation-settings: "wdth" 80` only when an inherited `font-stretch` causes conflicts.

To avoid layout shift, define fallback `@font-face` rules with `size-adjust` / `ascent-override` tuned against the real font. This matters most for Instrument Serif, which is far narrower than a default serif fallback; without an adjusted fallback, a headline can wrap very differently before the webfont loads.

## Native apps

Bundle the TTFs (OFL permits embedding) and register them: `UIAppFonts` on iOS, `ATSApplicationFontsPath` on macOS. Keep SF Pro for system controls, navigation bars, and anywhere Dynamic Type and platform conventions matter most. Use Instrument Serif for titles, onboarding, empty states, and brand moments, and Archivo for content where a custom voice helps. Scale both with `UIFontMetrics`/`.relativeTo` so Dynamic Type still works.

## When to depart from the pairing

- The product has an existing type system or brand guidelines. Preserve them.
- The language needs scripts the fonts lack. Both are Latin-only (Archivo also covers Vietnamese). Pick a superfamily that covers the language instead.
- A dense operational tool needs a lot of small UI text. Archivo can carry it alone. Instrument Serif may appear only in the app name or empty states, or not at all.
- The direction is deliberately monospace or technical. Archivo condensed can often deliver that voice. Add a mono face only when alignment of code or fixed-width data requires it.

State the departure and its reason in the design contract.
