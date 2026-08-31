# AI guide — Остров снова жив

This is the active guide for the project. Files under `reference/` describe SKYFORGE history only.

## Current Stage

Current scope is **STAGE 1 — FIRST REAL GAMEPLAY LOOP (v0.1.3)**. Setting is `TIMELESS COZY PRE-INDUSTRIAL / MEDIEVAL-INSPIRED ISLAND`.
Core manual loop: Find tree -> Chop with hand axe -> Tree falls -> Collect dropped logs -> Physical back cargo stack -> Deliver to sawmill storage -> Repair project progress -> First permanent restoration stage.
Workers, plank economy, bridge building, farm, and later zones are reserved for subsequent stages.

## Architecture

- `src/main.js` — bootstrap and guarded RAF loop; edge-to-edge viewport, renderer, light, camera orbit, UI lifecycle (title screen, pause modal, HUD, story cards), diagnostics, and player movement.
- `src/gameplay.js` — `GameplayManager`: save/load schema, contextual tree targeting & chopping, tree falling & stump replacement, dropped log scatter & pickup, player visible back cargo stack, sawmill delivery zone auto-unload, restoration transformation, and story beats.
- `src/collision.js` — centralized `CollisionRegistry` with typed static colliders (circles, OBB boxes, capsule segments), multi-iteration resolve, and 3D debug visualizer.
- `src/world.js` — irregular land silhouette (~94 × 70m), analytical ground/shore height, terrain volume, path splines, `isValidLandPosition` placement validation, surface type detection, stylized water, and distant atmosphere.
- `src/assets.js` — explicit production manifest, GLTF loading, bounds-based scale/grounding, collider registration, 34 harvestable tree slots entity data, 16 decor trees, scene placement, and player animation actions (`idle`, `walk`, `run`, `chop`, `cheer`).
- `src/input.js` — virtual joystick + multitouch horizontal/vertical camera orbit + contextual action input + desktop WASD/QE/RF/Space/right-click drag.
- `src/audio.js` — gesture-unlocked offline ambience, 12 CC0 WAV files (`sea`, `wind`, `birds`, `footsteps`, `axeHit`, `treeFall`, `pickup`, `deliver`, `construction`, `uiClick`), surface-type DSP acoustics, and phase-synchronized triggers.
- `src/style.css` / `index.html` — edge-to-edge mobile UI with title screen, HUD, contextual action button, story card, pause menu, and confirmation dialogs.
- `public/assets/` — selected runtime art (22 GLBs, 12 WAVs) and retained licenses.
- `android/` — Capacitor wrapper, immersive landscape activity, dark window background, and alpha signing.
- `.github/workflows/android.yml` — reproducible APK artifact delivery.

## Gameplay Conventions

- **Wood capacity:** Initial capacity = 10 logs.
- **Project goal:** 16 Wood to complete first restoration ("Починить навес и склад").
- **Tree durability:** 4 impacts (~2.5s) to fell a mature tree.
- **Axe impact sync:** Phase `0.38` in `1H_Melee_Attack_Chop` triggers hit sound, wood chip burst, and tree impulse.
- **Save system:** `localStorage` versioned JSON schema stores player position, carried wood, tree slot states, project progress, completed flags, and story state. Autosaves on milestones, pause, and visibility change.

## Debug and screenshots

`window.__GAME` exposes version, plain-object diagnostics (including carried wood, project progress, active tree, viewport metrics), quality setter, tree slots, collision registry, and gameplay manager.

Shot hashes:
- `#shot-zone1-forest`
- `#shot-zone1-sawmill`
- `#shot-zone1-coast`
- `#shot-zone1-overview`
- `#shot-colliders` (enables 3D wireframe colliders)

Debug flag: `?colliders=1` enables 3D wireframe colliders in regular gameplay.
