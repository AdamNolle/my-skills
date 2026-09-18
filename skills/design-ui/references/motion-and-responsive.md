# Motion and responsive composition

## Motion needs a job

| Job | Reference evidence | Transfer mechanism |
|---|---|---|
| Focus | Remy's colored active image among muted neighbors | Change emphasis while preserving selected identity |
| Explain an object | 60fps watch changes orientation as the story progresses | Align camera or object state with a meaningful chapter |
| Establish atmosphere | Immersive Garden's pale sculptural environment | A slow environmental layer behind readable content |
| Express an identity | Loop's object collage; Lando's portrait/helmet ribbons | Use a motif that belongs to the subject |
| Navigate a collection | Active Theory's spatial project panels | Preserve discoverable links and explicit selection controls |
| Reveal material | Cartier's transition from a stone to its jewelry scene | Let the selected object's color and light connect the states |

Describe a motion as **trigger → element → change → timing character → result → fallback**. “Smooth animation” is not a specification.

Suggested timing ranges for new work, not measurements of the references: immediate pressed feedback; roughly 120–200ms for a small control transition; 200–350ms for a panel or selection change; 450–900ms for a deliberate editorial reveal. Large continuous scenes require their own pacing. Do not stagger a large list until the whole interface becomes slow.

Use one shared easing vocabulary. Small controls can resolve quickly; large objects should have believable acceleration and deceleration. Springs should not bounce serious content merely because the library supports it. Animate transform and opacity where practical; verify rather than assume compositor performance.

## Four reusable interaction recipes

### 1. A collection with several representations

Inspired by Remy's slider, grid, and list.

- Keep one source of truth for project order, active project, filter, and mode.
- Derive each layout from those values. Changing mode should not silently reset selection.
- Give the active image full color and stronger contrast; quiet neighbors without making their labels unreadable.
- Use a transition between positions only when it clarifies continuity. Provide an immediate or short dissolve version.
- Implement real links to detail pages and a keyboard path to every item. A visually rich canvas must not be the only way to reach the work.
- On phones, choose a single-image composition or simple list; do not compress three desktop modes into tiny controls.

### 2. An object-led product story

Inspired by Apple, EMO, Cartier, and the 60fps watch.

- Sequence a small number of meaningful object views: overall form, relevant detail, evidence, comparison or specification.
- Connect each chapter to a user question. Rotation alone is not explanation.
- Keep the object grounded in a consistent light and camera system.
- If scroll progress drives the scene, ensure there is a readable state at each chapter. Avoid long stretches where text is too faint to read.
- Render the final still and essential copy without waiting for the full effect. Provide a static sequence when motion is reduced or the scene fails.

### 3. An expressive collage

Inspired by Studio Loop.

- Choose a small family of source objects related to the brand. Establish a focal cluster, an entry and exit direction, and planned empty space.
- Use depth, overlap, and scale differences to separate layers. Avoid random scatter.
- Keep navigation on a stable plane with sufficient contrast.
- Let selected type change character to emphasize meaning; preserve one reading hierarchy.
- Art-direct a new portrait arrangement for phones. A wide composition scaled down loses both text and objects.

### 4. A focused software demonstration

Inspired by Raycast and Forge.

- Show the real interface with a useful task and plausible data.
- Limit the demonstration to one action and consequence at a time.
- Keep chrome, command labels, shortcuts, and selected states consistent with the actual product.
- Separate atmospheric background motion from the interface demonstration. Let the interface remain readable when the background stops.
- A looping demo needs a useful poster and clear controls if its content is necessary to understand the product.

## Responsive decisions observed in the references

Phone captures in this research used a **390 × 844 browser viewport**, not a physical phone or a touch-device performance test.

- **Remy:** desktop multi-image modes gave way to a full-height portrait, compact top navigation, and a curved tick-mark treatment around the lower caption. The menu became a right-aligned list over a blurred image.
- **Loop:** the object collage was recropped for portrait. Its menu became a full-screen composition with staggered alignment, alternating serif/sans treatment, and multiple brand colors.
- **Locomotive:** header links collapsed to a menu; the hero's mixed typographic lockup wrapped into a narrow block. Lower editorial lists retained indentation and dividing rules.
- **Parakeet:** illustration moved above text, the long horizontal wordmark became a compact bird mark, and selected navigation remained visible.
- **Weird Folders:** the sampled collection became two columns with a compact bottom registration bar; the material icons retained their visual size and detail.
- **Forge:** a compact two-column title/object arrangement survived above a full-width explanation and actions. Responsive design does not always require stacking every element.

Treat these as evidence of purposeful composition, not universal breakpoint rules. Test intermediate widths, landscape, text enlargement, long labels, and menus where the new product needs them.

## Motion, input, and failure paths

Honor the user's reduced-motion preference by removing or replacing nonessential motion. For scroll scenes, ensure the static state contains the content; turning off a timeline must not leave text transparent or a panel offscreen. CSS can use `prefers-reduced-motion`, and JavaScript-driven scenes need the same preference applied to their state and renderer. [MDN guidance](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/prefers-reduced-motion).

Provide explicit alternatives to hover, drag, cursor effects, and sound. Keep audio opt-in. Give continuous animation a sensible pause path. Do not replace the system cursor in functional product interfaces by default.

Maintain native scrolling unless a specific experience justifies another model. Never let a scene trap the user between chapters, block the main action, or make browser back/forward confusing. A long branded loading sequence may be memorable once and frustrating on repeat visits.

## Performance follows the composition

Load the critical hero asset first; defer offscreen video and heavy scenes. Provide posters and fixed media dimensions. Pause rendering for inactive or hidden scenes. Avoid multiple full-screen blur layers, unnecessarily high pixel ratios, and several competing animation loops.

Measure on an appropriate device or profile before claiming performance. Current Core Web Vitals guidance uses LCP, INP, and CLS; good thresholds are LCP ≤2.5s, INP ≤200ms, and CLS ≤0.1, evaluated at the 75th percentile of visits. A local lab result is not field evidence. [Web Vitals](https://web.dev/articles/vitals).
