# Billing breakdown — minimal clarity pass (mockup)

Open `index.html` (or serve the folder) and use the floating bar: **Before / After**. Keys: `b` / `a`.

Before = exact live markup from huggingface.co (org `huggingface`, Sep 2026 period). After = same data, same compiled stylesheet, same utilities and type sizes as moon-landing.

## What changed in "After"

1. **One row grammar everywhere**: `label ···· meta | amount`. Section lines, group lines and item rows all use it, so the eye scans one way.
2. **Section lines carry their subtotal**: Compute Usage shows $43,915.66 (was nothing); Inference and Private Storage fold their single line into the header (removes one duplicate line each). Sizes follow the live Inference line: meta is base `text-gray-700`, amount is `text-smd font-semibold`.
3. **Groups nest one level in** (`pl-5`) under their section: Jobs / Spaces / Endpoints no longer read as siblings of Inference and Compute.
4. **Right-hand columns are fixed width from `md` up** (`md:w-48` hours, `md:w-28` amount, `tabular-nums`): hours and amounts align into true columns; the GRANT chip moves between them, so amounts stay flush right on every row. Below `md` the row wraps exactly like the live row does.
5. **Links lose the permanent underline** (underline on hover). Largest texture reduction in the section; chips, Active badge, GRANT chip and dotted leaders are untouched.
6. **Every category gets an icon**: Jobs → `IconTerminal`, Spaces → `IconSpaceMonochrome`, Endpoints → `IconInferenceEndpointsMonochrome`, all `text-gray-400` like the existing section icons. Jobs has no dedicated icon in moon-landing; Terminal is a stand-in.
7. **Lists hang off the same curve connector as Jobs**: Spaces and Endpoints rows sit inside the `15×28` curve block, so every group's detail reads as a child of its title.
8. **Spaces and Endpoints are collapsible**: rows are sorted by expense (highest first) and the top 7 show by default. The rest sit behind the profile page's list control from `NamespacedRepoList.svelte` (gradient hairlines fading in from the edges, bordered pill with caret, "Show 6 more spaces"), which flips to "Show less". The dashed-rule button from the payments section was tried first and dropped as off-system.
9. **Inference providers and Jobs hardware become carousels**: one row each, `overflow-x-auto no-scrollbar`. Edge fades are the existing `bg-linear-to-l/r from-white to-transparent` overlays, shown only on the side that has more content, so the first provider is never faded at rest.
   - **Cursor in the zone** (the Inference section, or the Jobs group): the carousel surface fills with `bg-gray-100`, the same hover fill the expand pill uses, on a `rounded-lg` wrapper (`-mx-2 px-2 -mt-1 -mb-1 py-1`, so nothing moves). The edge fades switch to `from-gray-100` so they stay seamless. Pure utilities: `group` on the zone, `group-hover:` on the wrapper and fades. A `shadow-sm` + ring lift was tried first and dropped for the fill.
   - **Wheel drives the carousel**: while the cursor is anywhere in the zone, the scroll wheel moves the row sideways and never the page, including at either end of the row. Moving the cursor out of the zone hands the wheel back to the page.

## Spacing

Same rhythm as live: `gap-4` between a section head and its content, `gap-6` between groups, `mb-4` under a group head, `mb-4 sm:mb-2.5` per row.

## Considered and dropped

- **Subtle group fill** (`bg-gray-50` container per section): tried, dropped on 2026-10-26 as too large a visual shift for the gain.
- **Collapsible provider list** (one line clamped at `max-h-12`, second line through a bottom fade, caret to open): built as an alternative to the carousel, dropped on 2026-10-26 in favour of the carousel.

## Not changed on purpose

Type sizes, chip and badge components, dotted leaders, the period header, the tabs row, the provider / hardware chip clusters, dark mode (mockup is light only). Checked at 1512px (one line per row, no truncation) and 1024px (long rows wrap, same as live).

## Mockup-only CSS

Only one state hook (`.bk-expanded`) and `hover:underline`, which is a stock Tailwind utility absent from this purged build. Everything visible uses classes that exist in the compiled stylesheet.

## Fonts

`fonts/` holds the 49 woff2 files the stylesheet references (Source Sans Pro, Charter, …), copied from huggingface.co. They must be self-hosted: huggingface.co serves them without a CORS header, so a cross-origin page falls back to the system font and every weight and width looks off.

## Regenerate

`python src/build.py src/live src/icons index.html` — `src/live/section.html` is the captured live section, `src/live/shell.html` the page chrome (scripts, props and tokens stripped).
