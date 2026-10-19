# ISWAS v8

Presentation build of **It Started With a Scream** for GitHub Pages.
Same pitch, same pages, same gallery, same interactions as v7 — the 3D lodge
has been re-lit and re-materialed for photorealism, and the renderer does less
work per frame than before.

## Upload

1. Create a public repo named `ISWAS_v8` (or push these files onto `main`).
2. Upload **the contents of this folder** to the repository root. Include `.nojekyll`
   (without it GitHub Pages hides the `__grok/` icon and manifest).
3. Settings → Pages → Deploy from branch → `main` → `/ (root)`.
4. The site will be at `https://jotsmedia.github.io/ISWAS_v8/`.

Paths are base-aware (`window.__ISWAS_BASE`), so a project-page subpath works.

## What changed in v8 — look

- **Physically based materials** on desktop. Walnut paneling, the herringbone floor,
  damask, leather and brass now have normal and roughness maps (`textures/pbr/`),
  so they catch light, show grain and reflect the lamps instead of reading flat.
- **Warm HDR environment** for reflections (lamp glows, velvet band, ceiling bounce)
  replaces the old cool-blue reflection box, so gilt and brass look like metal.
- **Molded gilt frames.** Each frame is a real swept ogee profile instead of 13 flat boxes.
- **Picture lights and glass.** Every portrait and plate gets a warm wash on the wall
  above it, a falloff across the print, and a glass pane that picks up reflections.
- **Cinematic post pipeline:** 4× MSAA HDR render, ambient occlusion (contact shadows
  in corners, under frames and furniture), bloom only on true highlights, Khronos
  Neutral tone mapping (keeps the photographs faithful), a light warm/cool grade,
  subtle lens falloff and dithering. Soft shadows from furniture and props.

## What changed in v8 — speed

- Fewer draw calls than v7 despite the extra detail (≈500 vs ≈760 per frame in the gallery, measured on the same view).
- Removed one point light per portrait (every light was paid for by every pixel).
- The mirror's full-scene reflection render is skipped whenever the mirror is off-screen
  and runs at half rate otherwise; it no longer re-renders shadows.
- Shadow maps update every other frame.
- Nine texture downloads that nothing displayed are gone (~0.9 MB on desktop), and 18 unreferenced texture files (~1.1 MB) are removed from the upload.
- **Adaptive quality.** The room starts at full quality and steps down
  (resolution → ambient occlusion → post) if frames run slow, and back up when there is headroom.
  Force a level for a presentation machine with `?quality=0…3` (3 = full).
- Phones and Save-Data / slow connections keep the lean v7 path (no post, Lambert
  materials, `m/` textures) and also benefit from the frame and mirror savings.

Do not strip the NUL bytes inside the TanStack hydration payloads in the HTML files.
