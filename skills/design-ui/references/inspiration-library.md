# Inspiration, tools, and craft library

These are sources the user collected, grouped by the job they help with. Each was visited on **2026-10-13**. Unlike [site-studies.md](site-studies.md), this is a working library, not a captured evidence study: no screenshots are bundled, and what these sites contain changes over time. Open the live source when a task depends on its current contents.

Use the library to widen the search for a direction or a technique, then turn what you find into an original mechanism, as [design-directions.md](design-directions.md) describes. Gallery entries are other people's work, and Apple slides are Apple's property. Study the relationships in them; do not lift assets.

## Quick routing

| Task | Start with |
|---|---|
| Find fresh visual references across disciplines | Recent, Toolfolio, the Pinterest-alternatives video |
| Product launch, keynote, recap, or feature-summary layout | Apple Summary Slides |
| App icon or macOS/iOS identity | The iOS / macOS App Icon Book, plus [icons.md](icons.md) |
| Coherent desktop/system icon family | Conflux Icon Theme |
| Era-specific or retro web styling | Evolution of the Web, Killed by Apple |
| Animation, gestures, color, typography craft on the web | Interfaces |
| Problem framing before visual work | Design Thinking Books |
| Refractive glass panels on the web | Outpace liquid-glass article, `samasante/liquid-glass` |
| Soft extruded (neumorphic) surfaces | Neumorphism.io |
| Shaders and effects in SwiftUI / React Native / web | MetalForge |
| Editor or app color-token theming | Zed Theme Builder |
| Exploded-view, teardown, and object-as-art product presentation | Artovision Exploded 128, Xreart, EMO Altar II |
| Small Mac utility identity and presentation | Vinz / Sydedock, ArtCraft Craft Apps |

## Design inspiration and galleries

**[Recent](https://recent.design)** is a frequently updated feed of new design work. The Design section can be filtered by Web, Interface, Branding, Motion, Illustration, Posters, and Print. Other sections cover Launch Videos, Websites, OG Images, App Screenshots, and App Icons, and there are Tools, Skills, and Jobs pages. *Use when* the brief needs contemporary references in a specific medium. The OG Images, App Screenshots, and Launch Videos sections are especially useful for launch assets. *Watch for* sponsored placements and trend convergence. Pick references for a reason, not for recency.

**[Apple Summary Slides](https://apple-summary-slides.vercel.app)** is Ricky Zhang's archive of the summary slides from Apple events and WWDC, 2019 to present. Slides can be browsed by event and by product, and a GitHub repo and Figma community file are linked. It is also listed on [Toolfolio](https://toolfolio.com/tools/apple-summary-slides) as an inspiration gallery. *Use when* designing a feature overview, release recap, bento grid, pricing or spec summary, or presentation deck. Study how many items fit one view, how each tile gets a single idea, and how product imagery, numerals, and short labels share hierarchy. *Caveat:* the slides are Apple's copyrighted material and are for study only.

**[Toolfolio](https://toolfolio.com)** is a directory of design, AI, and software tools, with categories such as Inspiration and Presentation Design. *Use when* looking for a specialist tool (motion, gradients, UI kits, mockups). Treat it as discovery, not endorsement; several listings are ads.

**[The iOS App Icon Book](https://flarup.shop)** is Michael Flarup's hardcover (and digital) art book of iOS icons, with essays and designer profiles. The same shop sells *The macOS App Icon Book* and an App Icon Stencil. *Use when* defining an app-icon direction. These books are the best offline reference for silhouette, material, lighting, and how much detail survives at small sizes. They pair with Parakeet and Weird Folders in [icons.md](icons.md).

**[Evolution of the Web](https://evolution-of-the-web.ybouane.com)** is an interactive timeline covering 35 years of web design. It renders one fictional site (the "Donut 404" shop) inside period OS chrome and browsers, beginning in 1991 with the "Document Web" (HTML, links, text), and a slider moves it through each era. *Use when* a brief asks for retro, era-accurate, or nostalgic styling, or to explain why a modern convention exists. It shows that each era's look came from its constraints, which is the lesson to transfer. Do not use it as a costume.

**Stop Using Pinterest: Favorite Alternatives** is a YouTube video about design-inspiration sources beyond Pinterest. *The exact video could not be located from its title on 2026-10-13; add its URL here when available.* Whatever the source, prefer curated boards over algorithmic feeds when collecting references.

## Design writing and typography

**[Interfaces](https://interfaces.dev)** is Jakub Krehel's design-engineering magazine. It publishes monthly deep-dive issues with interactive demos and code. Topics include shared layout animation, transition principles, "details that make interfaces feel better," gradients, OKLCH color, accessibility as craft, a web typography manual, and using AI as a design engineer. Three issues are free, and subscribers get agent skills and a resource library. *Use when* implementing motion, color, and polish details. Its approach matches [motion-and-responsive.md](motion-and-responsive.md) and [craft-and-build.md](craft-and-build.md).

**[Complete Guide to Instrument Serif](https://www.beautifulwebtype.com/instrument-serif/)** and **[Complete Guide to Archivo](https://www.beautifulwebtype.com/archivo/)** are Beautiful Web Type's specimen and feature pages for the house fonts. The rules for using them are in [typography.md](typography.md).

**[Design Thinking Books You Must Read](https://www.designorate.com/design-thinking-books/)** is Designorate's list of foundational reading on how designers frame problems, not on toolkits. It includes Dorst (*Design Expertise*, *Frame Innovation*), Cross (*Design Thinking*), Brown (*Change by Design*), Norman (*The Design of Everyday Things*), Lawson (*How Designers Think*), Simon (*The Sciences of the Artificial*), and the papers by Buchanan ("Wicked Problems in Design Thinking") and Rittel & Webber ("Dilemmas in a General Theory of Planning"). *Use when* the brief is ambiguous and the purpose and audience in the design contract need sharper framing.

## Web and UI design tools

**[Liquid glass for the web](https://glass.outpacestudios.com)** (Outpace Studios) argues that "blur isn't glass" and builds real refraction. A squircle dome lens is modeled with Snell's law at index ≈ 1.5, and the result becomes a displacement map applied through one SVG `feDisplacementMap`. The backdrop is rendered again as a counter-positioned copy and filtered with CSS `filter` so the effect works across browsers. Chromium gets three chromatic passes plus a specular pass; Safari runs a single software pass. The article documents Safari pitfalls: use `blob:` URLs instead of `data:` URIs in `feImage`, give the filter a fresh id on each rebuild, clip the copy to the lens box, and force sRGB. It also recommends one travelling lens rather than one per menu, honoring `prefers-reduced-motion` and `prefers-reduced-transparency`, and making glass an enhancement the UI works without.

**[samasante/liquid-glass](https://github.com/samasante/liquid-glass)** (MIT, React, zero dependencies) packages the same idea as a `<Glass>` component. Its `optics` controls include strength, depth, curvature, dispersion, frost, and sheen; geometry can be motion values; and video and canvas get a WebGL path. Chrome and Edge can bend the live page. Safari and Firefox need the `refract` copy mode, or they only frost and tint. The README does not cover reduced motion or contrast, so add both yourself.

*Glass guidance:* use it on one or two floating controls (navigation, a menu lens, a media control) over rich imagery, where refraction explains depth. Do not apply it to reading panels or dense UI. Guarantee text contrast with an opaque fallback, and test Safari performance on real content.

**[Neumorphism.io](https://neumorphism.io)** generates soft-UI CSS from a base color, size, radius, distance, intensity, blur, and shape (flat, concave, convex, pressed). The output is paired light and dark `box-shadow`s, for example `20px 20px 60px #bebebe, -20px -20px 60px #ffffff`. *Use when* a tactile, physical control (a dial, toggle, or hardware-like panel) is the point. *Caveat:* low-contrast extrusion is a known accessibility failure. Keep borders, labels, and focus and state changes legible without the shadows, and never use it as a whole-app surface system.

**[Zed Theme Builder](https://zed.dev/theme-builder)** ([announcement](https://zed.dev/blog/theme-builder)) is a browser-based editor for Zed themes. It shows a live replica of the Zed UI beside grouped color tokens (UI, syntax, collaboration). You can right-click an element to find its token and link tokens to one another, and themes import and export as JSON. *Use when* designing any editor, IDE, or dense app theme. It is a model of semantic token roles (surface, border, text, accent, status, syntax) and of checking every token in context.

**[MetalForge](https://metalforge.xyz)** is a library of shaders and animated effects (holographic cards, glass, orbs, liquid, text effects, loaders, buttons, and backgrounds) for SwiftUI, React Native, and the web. Each effect is added with a single line, some effects are free, and a beta MCP can add effects from an assistant. Check its license page before shipping. *Use when* a native app needs one signature material moment. Apply the same restraint as with glass: one effect with a reason, plus a static fallback for Reduce Motion.

**[Conflux Icon Theme](https://github.com/MoshiurRahmanAdib/Conflux-Icon-Theme)** (GPL-3.0) is a Linux icon theme whose goal is to make "all icons feel at home together" with a modern, neutral style. *Use when* designing a large icon family. Study how a shared grid, palette, and lighting harmonize hundreds of unrelated app marks. GPL licensing means it is study material, not a drop-in asset for proprietary work.

## Art and product design

**[Killed by Apple](https://killedbyapple.theden.sh)** is a fan-made graveyard of 142 discontinued Apple products, services, and features. Entries can be filtered by type (Hardware, Software, Service, Feature) and product line, and sorted by date, lifespan, or name. Each card has an emoji glyph, tags, a short wry obituary, a lifespan (e.g. 2006 → 2026 · 20 years), and source links. *Use when* building an archive, changelog, or timeline. It shows how a single repeated card with consistent metadata and a strong voice makes a long list browsable.

**[Exploded 128 Shadowbox Art](https://artovision3d.com/products/exploded-128-shadowbox-art)** (Artovision) depicts the Macintosh 128K as an exploded view. It is printed on layered optical-grade acrylic with physical depth, 12 × 15.5 × 2.5 in. *Use when* an exploded or layered product view is the hero idea. It shows that separating parts along a single axis with clear spacing makes an assembly readable. See [product-media-and-brand.md](product-media-and-brand.md) for when exploded 3D is justified.

**[Xreart](https://xreart.com)** sells framed teardowns of real devices (early iPhones, Game Boys, controllers, PS Vita). Components are hand-placed on printed templates, and labels name each part. It also sells templates and bespoke frames. *Use when* presenting hardware internals, specifications, or "what's inside" sections. The gallery-like arrangement of labeled components is a strong alternative to a spec table. The source wear is kept, which is the authenticity principle in [product-media-and-brand.md](product-media-and-brand.md).

**[Altar II by Electronic Materials Office](https://electronicmaterialsoffice.com)** is the current product page for a 4.75 mm fully mechanical keyboard with a detachable magnetic M-Dial and haptic feedback. EMO is already reference 02 in [site-studies.md](site-studies.md); this page is the live successor to that capture. The comparison section is worth studying: competing keyboards' thicknesses are ranked as a typographic list. Short headed claims ("Amazing feel.", "Durable design.") precede explanatory sentences.

**[Vinz](https://hivinz.com)** is the portfolio of a designer of small products for designers and developers, presented on a quiet dotted-grid page. Their main product, **Sydedock**, is a macOS side dock that combines clipboard, tasks, notes, hydration, habits, clocks, a focus timer, and assets as flyout panels from the screen edge. *Use when* designing a Mac utility or menu-bar/edge app. Notice the compact product listings, each with an icon, one-line purpose, and a screenshot row.

**[ArtCraft](https://getartcraft.com)** is an open-source studio (macOS, Windows, web) for controllable AI image and video work. Features include image to location, 2D/3D compositing, image to 3D mesh, character and camera posing, and background removal. Its **[Craft Apps](https://getartcraft.com/apps)** family (PhotoCraft, VectorCraft, FilmCraft, LightCraft, and more) shares a naming system. *Use when* naming and presenting a suite of tools, or when original campaign art, cutouts, or 3D blocking are needed for an asset plan. The page's numbered section headers ("01/Crafting features") are another example of an editorial index system.
