# Animated Landing Page Implementation & Asset Guide

> Purpose: implement an animated landing page inside an existing repository, using the repository's real architecture and a full landing-page reference image supplied by the user.
> Primary visual authority: the user's full landing-page reference image.
> Primary implementation authority: `AGENTS.md` and the repository's existing architecture, stack, conventions, and tooling.
> Final media workflow: build the landing page first with production-geometry dummy image/video assets, verify it, then let the user replace those files with final generated media.

## 0. Non-negotiable gates

This workflow has two mandatory inputs before implementation can begin:

1. a valid repository/workspace containing `AGENTS.md`, and
2. a user-supplied full landing-page reference image/screenshot.

If either is missing, STOP.

Do not continue by guessing.
Do not invent a visual direction.
Do not search for a substitute landing-page template.
Do not create a generic SaaS landing page from memory.
Do not treat a text description or unrelated inspiration as a replacement for the required full landing-page reference image.

The user must provide the actual full-page visual reference that the landing page is expected to follow.

## 1. Read the repository before touching the landing page

The first implementation step is always repository understanding.

Read `AGENTS.md` first.

Then inspect the repository documents and source needed to understand:

- repository rules,
- architecture boundaries,
- existing tech stack,
- framework and runtime,
- routing/navigation model,
- component structure,
- styling system,
- state/data patterns when relevant,
- asset conventions,
- build and package tooling,
- testing conventions,
- browser/E2E tooling,
- existing product/brand documentation,
- existing landing-page implementation if one already exists.

Do not assume the project is a single-file HTML prototype.
Do not force the project into `index.html`.
Do not replace an established framework or architecture just to make the animated landing easier to implement.

Implementation MUST follow the stack and architecture that already exist in the repository.

Examples:

```text
Existing Next.js project
-> implement using the existing Next.js routing/components/styles/build system

Existing React/Vite project
-> implement inside the current React/Vite architecture

Existing Nuxt/Vue project
-> use the established Nuxt/Vue structure

Existing static HTML project
-> only then keep the implementation static if that is the repository's actual architecture
```

The landing page is a feature inside the repository, not a reason to redesign the repository architecture.

## 2. Source-of-truth hierarchy

Use the following authority order.

### 2.1 Repository architecture authority

`AGENTS.md` and the actual repository decide HOW the landing page is implemented.

They control:

- file placement,
- component boundaries,
- framework usage,
- dependency rules,
- code conventions,
- testing requirements,
- build/runtime constraints,
- architectural boundaries.

### 2.2 User reference image authority

The user's full landing-page reference image decides WHAT the landing page should look like.

It controls, as far as visible in the reference:

- section order,
- composition,
- layout rhythm,
- spacing,
- alignment,
- typography hierarchy,
- visual density,
- media placement,
- card/container geometry,
- background treatment,
- color balance,
- depth/layering,
- masks/crops,
- decorative visual language,
- overall art direction.

### 2.3 Product documentation authority

PRD/DESIGN/product documentation controls project-specific content and behavior that are not purely visual.

Examples:

- real product copy,
- brand name,
- navigation destinations,
- actual CTA behavior,
- accessibility requirements,
- application routing,
- project-specific data/content.

If there is a conflict:

```text
architecture question -> follow AGENTS.md / repository
visual composition question -> follow user reference image
product behavior/content question -> follow PRD/DESIGN/project docs
```

Do not casually rewrite PRD or architecture merely because the reference looks different.
Only update supporting design/asset documentation when needed to record the implementation faithfully.

## 3. The full landing-page reference image is mandatory

The agent MUST inspect the user-supplied full landing-page reference before implementation.

The reference must be sufficiently complete to understand the intended page structure.

If the image is:

- missing,
- only a cropped fragment when the rest of the page matters,
- unreadable,
- too low resolution to understand major layout decisions,
- or clearly incomplete,

STOP and ask the user for a usable full-page reference image.

Do not fill missing sections with invented layouts.

A URL may be useful as supplementary context, but it does not replace the required user-provided full-page reference image unless the user explicitly says the URL itself is the reference and the agent can inspect the complete page reliably.

## 4. Analyze the reference before coding

Before implementation, convert the reference image into a concrete page map.

Record at minimum:

- page-wide background treatment,
- header/navigation geometry,
- hero composition,
- section order,
- approximate section heights,
- content width,
- horizontal gutters,
- vertical spacing rhythm,
- typography hierarchy,
- image/video slots,
- masks and crop behavior,
- cards/panels/surfaces,
- decorative layers,
- foreground/background depth,
- CTA placement,
- repeated component patterns,
- footer treatment,
- responsive assumptions that can be reasonably derived.

For each section, explicitly map:

```text
Reference section
-> repository component/page area
-> required HTML/UI structure
-> required media slot(s)
-> interaction/motion responsibility
```

The implementation should be recognizably faithful to the supplied reference image.

Do not reinterpret the image into a generic design language.
Do not simplify a distinctive composition into ordinary centered hero + feature cards unless that is what the supplied reference actually shows.

## 5. Resource gathering now focuses on implementation, not design discovery

Do NOT perform broad template/inspiration hunting to choose a new visual direction.
The visual direction has already been supplied by the user through the full-page reference image.

Resource gathering should answer implementation questions such as:

- how to reproduce the reference's scroll behavior in the repository's existing stack,
- whether the current framework already has suitable animation utilities,
- whether GSAP/ScrollTrigger, Motion, Three.js, WebGL, CSS scroll timelines, canvas, or another focused dependency is appropriate,
- how to implement pinned/sticky scenes,
- how to implement masks/reveals/camera-like movement,
- how to map image/video media to the required geometry,
- how to optimize large media,
- how to preload/poster/fallback video safely,
- how to respect reduced motion,
- how to keep animation responsive,
- how to avoid layout shift,
- how to integrate with the existing build/runtime architecture,
- how to test the implementation with the repository's existing Playwright/browser setup.

Prefer official documentation and implementation references for the actual stack being used.

The research question is:

```text
How do we implement THIS supplied reference faithfully in THIS existing repository?
```

Not:

```text
What other landing-page design should we use instead?
```

## 6. Dependency policy

Use the repository's existing dependencies when they can reproduce the reference faithfully.

A new focused dependency may be added only when:

- repository rules allow it,
- it materially improves implementation fidelity or maintainability,
- the same result would otherwise require worse custom code,
- it fits the current stack and architecture.

Possible examples when justified:

- GSAP / ScrollTrigger,
- Motion / Framer Motion,
- Three.js,
- WebGL / shader helpers,
- Lenis,
- image-sequence helpers,
- focused media utilities.

Do not add a dependency merely because it is fashionable.
Do not bypass repository architecture to use it.

## 7. Build the complete landing page BEFORE final media exists

The landing page must be implemented to production geometry before the user generates final image/video assets.

The initial implementation phase must include:

- final section structure,
- final layout,
- final component hierarchy,
- final media containers,
- final responsive geometry,
- final masks/crops,
- final z-index/layering,
- final image/video DOM ownership,
- final motion orchestration,
- final fallback/poster behavior,
- production-path dummy assets.

The page should already look structurally like the supplied reference.

The only intentionally temporary part should be the CONTENT of the dummy image/video files.

## 8. Dummy image/video assets must occupy the real final slots

For every final image or video slot:

- create the final production file path immediately,
- create a valid lightweight dummy file at that path,
- use that file in the real final component,
- give the component final geometry,
- give it final crop/fit behavior,
- give it final mask/radius,
- give it final z-index/layering,
- give video its final autoplay/loop/muted/playsinline/poster behavior,
- verify responsive behavior with the dummy file.

The dummy asset MUST be the primary visual in the final media slot.

Do NOT:

- keep a legacy static illustration visible as the real visual,
- hide the dummy behind an old design,
- build a fake HTML illustration that will later need to be removed,
- wait for final generated media before creating the final component structure,
- use a temporary path that later requires code edits.

Target contract:

```text
landing implementation complete
+ dummy image/video already visible in final slots
+ motion/scroll implementation already wired
+ Playwright verification passes
-> user replaces files only
-> refresh/restart
-> final media appears without layout/source-code changes
```

## 9. Asset manifest

Create an asset manifest based on the supplied reference and actual implemented components.

Do not invent arbitrary asset counts.
Create only the media actually required by the reference-driven implementation.

Every final production media file should follow repository asset conventions.
If the repository uses a public/static/assets directory, use that established location rather than forcing a new root layout.

Record at minimum:

```markdown
| ID | Type | Final file | Reference area | Used in | Source ratio | Display ratio | Fit/crop | Motion |
|---|---|---|---|---|---|---|---|---|
```

The manifest must map each generated asset back to the exact visual area in the user's reference image.

## 10. Reference fidelity contract for generated media

The agent is NOT allowed to invent the visual direction of generated image/video assets.

Every prompt must be derived directly from:

1. the user's full landing-page reference image,
2. the exact target region/section visible in that reference,
3. the implemented component geometry,
4. the project's actual content/brand when content substitution is necessary.

The user's reference image is the visual source of truth.

Prompts must preserve all observable characteristics that matter, including when visible:

- composition,
- subject placement,
- perspective,
- camera angle,
- lighting direction,
- background color/treatment,
- palette,
- depth,
- focal hierarchy,
- negative space,
- crop-safe area,
- visual density,
- material/style language,
- relation to surrounding UI.

Do not invent:

- a different scene,
- a different camera angle,
- extra objects,
- extra characters,
- unreferenced lighting concepts,
- arbitrary 3D environments,
- unrelated decorative motifs,
- a generic AI art style,
- a visual concept sourced from a different website.

If a visual detail cannot be determined confidently from the user's reference, do not silently fabricate it.
Either keep that part neutral/minimal or ask the user when it materially affects fidelity.

## 11. Project-specific content substitution

The reference controls composition and art direction, but project-specific content may need to replace reference-specific content.

Example:

```text
reference has automotive subject in a large right-side hero visual
project requires an LMS visual

Keep:
- exact visual slot,
- scale,
- framing,
- perspective,
- negative space,
- lighting balance,
- crop behavior,
- relationship to hero copy.

Replace only:
- automotive subject -> LMS-relevant subject explicitly supported by project requirements/user instruction.
```

Substitution must be conservative.
Do not redesign the composition while replacing the subject.

If the correct semantic replacement is ambiguous, ask the user instead of inventing it.

## 12. Image prompt requirements

Each image prompt must explicitly identify the user reference as its authority.

Use a structure like:

```markdown
# I01 - <Asset Name>

## Output
- Final file: `<repository-final-path>`
- Used in: `<component/section>`
- Source ratio: `<ratio>`
- HTML/component display ratio: `<ratio>`
- Fit/crop: `<object-fit/object-position/mask details>`

## Mandatory visual reference
Use the user-supplied full landing-page reference image as the visual source of truth.
Target reference area: `<describe exact section/region>`.

## Preserve from reference
- composition
- framing
- subject scale and position
- perspective/camera angle
- lighting
- palette/background
- depth
- negative space
- visual density
- crop-safe focal area

## Project substitution
Only replace reference-specific subject/content where required by the project.
Describe the exact substitution.
Do not change composition while substituting content.

## Component contract
- exact component size/ratio
- exact background
- radius/mask
- crop behavior
- surrounding UI

## Google Flow image prompt
<complete prompt that follows the supplied reference faithfully>

## Negative constraints
- no alternate composition
- no extra objects
- no new environment
- no arbitrary camera angle
- no generic AI redesign
- no unwanted gradient/tint/vignette unless present in the reference
```

If Google Flow or another generator supports attaching a reference image, the prompt should explicitly instruct the user to attach/use the supplied landing-page reference or an approved crop derived from it.

## 13. Image-first workflow for video

For designed video assets, use image-first generation by default.

```text
user full-page reference
-> identify exact target region
-> write reference-faithful still-image prompt
-> user generates still using the reference
-> approve still against the reference image
-> use approved still as video source
-> write video-from-image prompt
-> generate final video
-> replace dummy media file
```

The still image is not an opportunity to reinterpret the reference.
It exists to lock the reference composition before motion is added.

## 14. Video prompt requirements: do not invent motion

A static landing-page image does not always define motion clearly.

Therefore the agent must distinguish between:

1. motion that is clearly implied by the supplied reference/implemented interaction,
2. motion explicitly requested by the user,
3. motion that would be pure invention.

The agent may use categories 1 and 2.
The agent must NOT invent category 3.

If meaningful motion direction cannot be inferred from the supplied reference and repository behavior, STOP before writing a cinematic video prompt and ask the user for one of:

- a motion/video reference,
- an animation description,
- permission to use restrained neutral motion.

Do not invent dramatic camera moves just because the page is called animated.

When motion is known, use this structure:

```markdown
# V01 - <Asset Name> From Approved Image

## Output
- Final file: `<repository-final-path>`
- Used in: `<component/section>`
- Duration: `<duration>`
- Loop: `<yes/no>`
- Audio: none unless explicitly required

## Visual authority
Use the approved still image as the visual source of truth.
The approved still must already match the user-supplied landing-page reference.

## Preserve exactly
- composition
- framing
- subject identity
- subject position/scale
- perspective
- background
- palette
- lighting
- crop-safe region

## Motion authority
Motion source: `<user instruction / motion reference / clearly implied browser interaction>`

## Motion prompt
Animate only the approved motion direction without redesigning the scene.

## Negative constraints
- no new subjects
- no new camera angle unless explicitly authorized
- no new environment
- no composition redesign
- no random particles/effects
- no lighting redesign
- no unexpected zoom/orbit/pan
```

The video prompt animates the approved image; it does not redesign it.

## 15. Browser motion vs generated media motion

Use repository code for interaction that should respond to the browser/user:

- scroll choreography,
- sticky/pinned sections,
- transforms driven by scroll,
- text reveals,
- masks,
- parallax/depth,
- cursor interaction,
- section transitions,
- Three.js/WebGL scenes,
- responsive behavior,
- interactive component states.

Use generated video for authored visual motion that is explicitly supported by the reference or user-provided motion direction.

Do not bake interactive behavior into video when code should control it.
Do not invent video motion simply to avoid implementing interaction in the real stack.

## 16. Background and crop contract

Generated media must integrate with the actual component.

Record and preserve:

- exact background color,
- source ratio,
- display ratio,
- `object-fit`,
- `object-position`,
- crop-safe region,
- mask/radius,
- responsive behavior.

If the reference uses a seamless white background and the component is `#FFFFFF`, generated media should also use `#FFFFFF` when alpha is unavailable.

Do not allow generation to add unwanted:

- gray tint,
- blue tint,
- vignette,
- gradient,
- fog,
- edge darkening,
- extra outer card/frame.

If the repository UI supplies the outer frame, the generated asset should not duplicate it unless the user reference explicitly shows the frame inside the media itself.

## 17. Mandatory implementation verification before media handoff

The landing page must pass real-browser verification while still using dummy assets.

This proves that final media replacement will not trigger another implementation pass.

Before handoff, verify:

- reference-driven section order is correct,
- layout is recognizably faithful to the supplied reference,
- media slots match the reference geometry,
- dummy image/video is actually visible in every final media slot,
- no legacy/fake visual is hiding the real media slot,
- crop/fit/mask/radius are correct,
- scroll/motion choreography works,
- sticky/pinned sections enter/release correctly,
- responsive layouts are usable,
- reduced-motion mode is usable,
- media loading does not cause harmful layout shift,
- poster/fallback states work,
- all actionable controls work,
- no critical runtime/console/resource error breaks the landing page.

## 18. Mandatory Playwright full E2E gate

Playwright verification is mandatory before the agent reports the landing implementation ready for final media generation/replacement.

First reuse the repository/workspace's existing Playwright installation or browser automation setup.
Do not install another Playwright copy when a usable one already exists.

Verification must use the actual application through the repository's normal runtime/serve command.

At minimum test:

- landing route loads successfully,
- all landing sections are reachable,
- visual/media slot geometry at desktop,
- relevant responsive breakpoint(s),
- all visible landing links/buttons/controls,
- scroll-driven transitions,
- pinned/sticky sections,
- dummy video autoplay/loop/poster/fallback behavior when relevant,
- image/video crop and focal position,
- WebGL/Three.js/canvas initialization when used,
- resize behavior,
- reduced-motion behavior,
- network/media failure fallback,
- no unintended overflow,
- no critical layout shift,
- no unresolved critical console/runtime error,
- any broader application flows required by the repository/PRD.

Any failed mandatory check is a completion blocker.
Fix it and rerun regression before handoff.

## 19. Required handoff artifacts

The exact filenames follow repository conventions, but the handoff should normally include:

```text
implemented landing page in the existing stack
asset manifest
prompts/
  image/*.md
  video/*.md
production-path dummy media
Playwright verification evidence/notes when the repository expects them
```

Do not create a parallel standalone HTML prototype unless the repository architecture explicitly calls for one.

## 20. Completion criteria

Do not report the animated landing workflow ready until all of these are true:

- `AGENTS.md` was read first,
- repository architecture/stack was understood,
- implementation follows the existing stack,
- a user-supplied full landing-page reference image exists,
- no alternate template/design direction was invented,
- resource gathering focused on HOW to implement the supplied reference in the existing stack,
- the complete landing structure is implemented,
- dummy image/video assets occupy the real final slots,
- final media paths are already wired,
- image prompts follow the supplied reference rather than an invented design,
- video prompts use approved reference-faithful still images,
- ambiguous video motion was not invented,
- Playwright full E2E verification passes,
- replacing final media should require only replacing files and refreshing/restarting the app.

## Preferred end-to-end workflow

```text
Receive repository/workspace
-> read AGENTS.md first
-> inspect architecture, stack, product docs, and existing implementation
-> require user-supplied full landing-page reference image
-> if reference missing/incomplete: STOP
-> map reference sections and media slots
-> research implementation techniques for the EXISTING stack only
-> implement landing in the existing architecture
-> create production-path dummy image/video files
-> wire dummy media into the real final slots
-> finish responsive + scroll/motion behavior
-> create reference-mapped asset manifest
-> write image prompts that faithfully follow the user's reference
-> generate/approve reference-faithful stills
-> if motion is ambiguous: ask user for motion direction/reference
-> write video-from-image prompts using approved stills
-> reuse existing Playwright
-> run mandatory full E2E verification
-> fix and rerun until pass
-> hand prompts to user
-> user generates final media
-> user replaces dummy files only
-> refresh/restart app
-> run final Playwright regression with real media
```
