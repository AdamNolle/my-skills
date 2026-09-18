# Authentic product media, 3D studies, and brand artifacts

Use this reference when a design depends on a real physical product, product footage, isolated product cutouts, explanatory 3D, logos, favicons, or app/site identity.

## Source truth before spectacle

For a named real product, choose assets in this order:

1. Suitable user-provided originals.
2. Official or independently photographed media with clear reuse rights.
3. A faithful edit of a licensed real source: background extraction, cleanup, crop, exposure, color, and restrained compositing.
4. Original 3D work for explanatory or otherwise unavailable views.
5. Generated imagery only when the required visual does not exist or the brief explicitly calls for interpretation.

Keep a source record for every shipped third-party asset: source page, creator, license, original URL, and material edits. Download the original resolution from the source page rather than a search thumbnail. Verify that the file depicts the correct model, generation, finish, and period.

Do not assume that an official-looking image is licensed for reuse. Research screenshots, retailer galleries, social posts, press coverage, and video-platform uploads are references until their rights allow production use.

## Edit real media without replacing it

Background removal and cleanup should preserve the product rather than redraw it.

- Treat the real photograph as the edit target. Preserve geometry, branding, control placement, reflections, surface wear, and historically relevant details.
- Request a genuine alpha channel for isolated output. Inspect edges at 100% and on both light and dark fields; remove halos, color fringing, clipped transparent materials, and accidental shadow remnants.
- Keep a non-destructive original and save the edited derivative under a new descriptive filename.
- Retain a realistic contact shadow only when the final composition needs grounding. A floating cutout and a grounded product shot are different art-direction choices.
- Match crops to the product's story. Controls and material transitions should remain visible; phone crops may require a different source or art-directed derivative.
- Optimize final web assets to AVIF or WebP when supported, preserve intrinsic dimensions, and keep an accessible fallback when the delivery context needs one.

For transparent glass, acrylic, smoke, mesh, or reflections, expect a harder extraction. Prefer a source photographed on a separable background; use manual masking or image editing when automatic removal destroys translucency.

## Use real product video deliberately

Footage should prove motion, scale, use, or material behavior—not act as a generic moving background.

- Ship only footage with suitable reuse rights or footage supplied by the user.
- Choose one or two concise moments: operation, opening, assembly, surface response, or human scale.
- Provide a useful poster frame and explicit dimensions. Use muted `autoplay` and `playsinline` only for short ambient loops; meaningful narration or demonstrations need controls and captions/transcript as applicable.
- Do not hide essential information exclusively in motion. Respect reduced motion by pausing or replacing ambient playback with the poster frame.
- Encode efficient web formats and sizes. Prefer a short WebM/MP4 pair when browser support requires it; avoid shipping a large source master.
- Attribute licensed footage near the media or in a clearly reachable credits block.

## Build explanatory Blender assets

Use Blender or another 3D tool when depth, assembly, or viewpoint materially improves understanding.

Before modeling, define the claim: product portrait, exploded assembly, cutaway, material study, or motion diagram. Gather orthographic or multi-angle references and any known dimensions. Model only the detail required by the final camera and output size.

For exploded views:

- Keep the assembled silhouette recognizable and the separation axis easy to follow.
- Group real subsystems where evidence exists; do not invent precise internal structure from appearance alone.
- Preserve fastening and enclosure logic. Parts should separate in plausible directions without intersecting.
- Use a restrained material and lighting system so hierarchy comes from spacing and scale rather than arbitrary color.
- Label a simplified model as “interpretive,” “diagrammatic,” or “based on public references.” Do not present it as manufacturing documentation.
- Save the editable scene, construction script when procedural, and final renders outside transient directories. Render alpha when the page composition supplies the background.

A still exploded view is often enough. Add a turntable or assembly animation only when motion explains order, spatial relationships, or access. Provide a reduced-motion still.

## Design logos and favicons as separate scales

Brand artifacts are part of the interface, not a deployment afterthought.

- Reuse an official logo only when the project is authorized to use it and the asset is reliable. Editorial tributes and unofficial product studies should use an original publication mark rather than imitating the product company's logo.
- Define the identity system: full wordmark or lockup, compact site mark, favicon, and app icon when applicable. These can share a concept without being the same drawing at every size.
- A favicon needs a bold silhouette, simple negative space, and useful contrast at 16px and 32px. Remove small text, thin internal rules, and decorative detail that collapses when downsampled.
- Inspect the compact mark on light, dark, pinned-tab, and browser-chrome contexts. Create monochrome and maskable variants when the platform uses them.
- Keep clear space, optical centering, corner treatment, and background shape deliberate. Test 16, 32, 64, 180, and 512px exports when those sizes are part of delivery.
- Use vector originals for simple marks and glyphs. Raster or 3D identity art may support launch surfaces, but it does not replace a crisp small-size mark.
- Wire the actual favicon and metadata before handoff. Remove starter icons and generic framework marks.

## Review and handoff

Check the final page with its actual media loaded. Review crop, edge quality, transparency, poster frames, playback policy, color consistency, asset weight, logo clear space, and favicon recognition. Name any interpretive 3D assumptions and preserve the source/attribution record with the project.
