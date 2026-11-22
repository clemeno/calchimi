# Which 3D library fits a zoneless Angular app for third-person click-to-move play?

Research for [#3](https://github.com/clemeno/calchimi/issues/3) (map [#1](https://github.com/clemeno/calchimi/issues/1)). Gathered 2026-11-22.

Candidates: raw **Three.js**, **angular-three** (NGT), **Babylon.js**. Target: latest Angular (standalone, zoneless, signals), PC moved by tapping or clicking the ground, shoulder orbit camera, mouse and touch.

## Versions at a glance (2026-11-22)

| Package | Latest | Published | Angular peer dep | Source |
|---|---|---|---|---|
| `@angular/core` | 22.3.0 (22.0.0 shipped 2026-06-03) | 2026-11-18 | n/a | [npm](https://registry.npmjs.org/@angular/core) |
| `three` | 0.187.0 (r187) | 2026-10-30 | none | [npm](https://registry.npmjs.org/three) |
| `angular-three` | 4.2.4 | 2026-07-15 | `@angular/core >=20.0.0 <22.0.0`, `three >=0.157.0 <0.183.0` | [npm](https://registry.npmjs.org/angular-three) |
| `@babylonjs/core` | 9.37.1 (9.0.0 shipped 2026-03-26) | 2026-11-18 | none | [npm](https://registry.npmjs.org/@babylonjs/core) |
| `camera-controls` | 3.1.2 | 2025-11-17 (repo pushed 2026-10-21) | none (`three >=0.126.1`) | [npm](https://registry.npmjs.org/camera-controls), [repo](https://github.com/yomotsu/camera-controls) |
| `@recast-navigation/three` | 0.44.0 | 2026-10-30 | none (`three 0.x.x`) | [npm](https://registry.npmjs.org/@recast-navigation/three) |
| `three-pathfinding` | 1.3.0 | 2024-05-17 | none (`three 0.x.x`) | [npm](https://registry.npmjs.org/three-pathfinding) |
| `@babylonjs/addons` (navigation plugin V2) | 9.37.1 | 2026-11-18 | none | [npm](https://registry.npmjs.org/@babylonjs/addons) |

## Comparison by criterion

### 1. Maintenance and Angular-version support

- **Three.js**: about one release every two months (r183 2026-02-18, r184 04-16, r185 06-25, r186 09-08, r187 10-30) ([npm time data](https://registry.npmjs.org/three)). Repo pushed 2026-11-22, about 119k stars ([GitHub](https://github.com/mrdoob/three.js)). It has no Angular dependency, so it works with any Angular version.
- **angular-three**: **does not support Angular 22.** Every release from 4.0.4 to 4.2.4 caps `@angular/core` at `<22.0.0` and `three` at `<0.183.0` (r182, December 2025) ([npm](https://registry.npmjs.org/angular-three)). Angular 22 has been out since 2026-06-03. Issue [#91 "Angular 22 support"](https://github.com/angular-threejs/angular-three/issues/91) has been open since 2026-09-14. The only reply comes from a contributor, who reports that the upgrade needs an Nx 24 migration and leaves lint errors unresolved. No maintainer has answered. Last release 2026-07-15, last commit 2026-08-08, 303 stars ([GitHub API](https://github.com/angular-threejs/angular-three)). v4 states "a minimum of Angular 20 and Three.js 174+" and recommends `provideZonelessChangeDetection()` ([v4 post, 2026-01-05](https://angularthree.org/blog/v4/)). Another open renderer bug: [#95](https://github.com/angular-threejs/angular-three/issues/95) (projected content inside a toggled `@if` loses its children).
- **Babylon.js**: 75 stable 9.x releases since 2026-03-26, latest 2026-11-18 ([npm](https://registry.npmjs.org/@babylonjs/core)). Repo pushed 2026-11-21, about 26k stars ([GitHub](https://github.com/BabylonJS/Babylon.js)). It has no Angular dependency.

### 2. Ground and object picking with pointer events (mouse and touch)

- **Three.js**: you build it from `Raycaster.setFromCamera` plus `intersectObject` on DOM `PointerEvent`s, which cover mouse and touch. Telling a tap from a drag (tap moves the PC, hold-drag orbits) takes a few lines of your own: compare the pointerdown and pointerup positions. `three-mesh-bvh` 0.9.18 (2026-11-03) speeds up raycasts if they ever get slow ([npm](https://registry.npmjs.org/three-mesh-bvh)). Primitive shapes do not need it.
- **angular-three**: template outputs (`click`, `pointerover`, …) on `ngt-*` elements, backed by a raycasting event system with pointer capture ([events.ts](https://github.com/angular-threejs/angular-three/blob/main/libs/core/src/lib/events.ts), [homepage samples](https://angularthree.org/)).
- **Babylon.js**: built in. `scene.onPointerObservable` emits `POINTERTAP`, which "is fired when the object has been touched and released without drag" ([pointerEvents.ts](https://github.com/BabylonJS/Babylon.js/blob/master/packages/dev/core/src/Events/pointerEvents.ts)). "Without drag" means under `InputManager.DragMovementThreshold = 10` px, and there is also `LongPressDelay = 500` ms ([scene.inputManager.ts](https://github.com/BabylonJS/Babylon.js/blob/master/packages/dev/core/src/Inputs/scene.inputManager.ts)). `scene.pick(x, y)` then returns the hit. This gives tap-vs-hold with no extra code.

### 3. Click-to-move pathfinding (navmesh)

- **recast-navigation-js** (0.44.0, 2026-10-30, repo pushed the same day) is the live option for both engines ([repo](https://github.com/isaac-mason/recast-navigation-js)). `@recast-navigation/three` builds a navmesh **at runtime from `THREE.Mesh` objects** with `threeToSoloNavMesh`, `threeToTiledNavMesh`, or `threeToTileCache`. The tile cache supports temporary obstacles ([README](https://github.com/isaac-mason/recast-navigation-js/blob/main/packages/recast-navigation-three/README.md)), which would cover Setups placed later. A Plot built from primitive shapes then needs no Blender step.
- **three-pathfinding** 1.3.0: "This library does not build navigation meshes for you". You bake the navmesh in Blender or another tool and load it as glTF or OBJ ([README](https://github.com/donmccurdy/three-pathfinding/blob/main/README.md)). The repo still gets commits (2026-10-06), but there has been no release since 2024-05-17. It is ruled out because of the baking step.
- **Babylon navigation plugin**: V2 ships in `@babylonjs/addons` 9.37.1 as `CreateNavigationPluginAsync` → `RecastNavigationJSPluginV2`, built on recast-navigation-js ([factory source](https://github.com/BabylonJS/Babylon.js/blob/master/packages/dev/addons/src/navigation/factory/factory.single-thread.ts), [announcement](https://forum.babylonjs.com/t/navigation-plugin-v2-is-here/60751)). By default it **loads `@recast-navigation` 0.43.0 from unpkg at runtime** through an import map. To bundle it from npm instead, pass `options.instance` ([common.ts](https://github.com/BabylonJS/Babylon.js/blob/master/packages/dev/addons/src/navigation/factory/common.ts); the same code ships in the 9.37.1 tarball). The older V1 `RecastJSPlugin` in core is still the documented one ([docs](https://doc.babylonjs.com/features/featuresDeepDive/crowdNavigation/createNavMesh)). Its click-to-move pattern is `computePath(start, getClosestPoint(dest))`.
- **angular-three**: has no navmesh package. You would use `@recast-navigation/three` directly.

### 4. glTF skinned character with idle and walk animations

- **Three.js**: `GLTFLoader` + `AnimationMixer` + `clipAction` + `crossFadeTo`. The official r187 example does exactly this, idle to walk to run ([webgl_animation_skinning_blending.html](https://github.com/mrdoob/three.js/blob/r187/examples/webgl_animation_skinning_blending.html)).
- **angular-three**: soba `gltfResource` and the `animations` helper ([homepage samples](https://angularthree.org/)).
- **Babylon.js**: `ImportMeshAsync` from `@babylonjs/loaders/glTF` returns `animationGroups: AnimationGroup[]` ([sceneLoader.ts](https://github.com/BabylonJS/Babylon.js/blob/master/packages/dev/core/src/Loading/sceneLoader.ts)).

All three handle this; it does not separate the candidates.

### 5. Orbit camera: min/max distance, pinch zoom, camera collision against walls, shoulder offset

- **Three.js + `OrbitControls`** (r187): `minDistance`/`maxDistance`, and `touches = { ONE: TOUCH.ROTATE, TWO: TOUCH.DOLLY_PAN }` by default, so pinch already zooms ([OrbitControls.js](https://github.com/mrdoob/three.js/blob/r187/examples/jsm/controls/OrbitControls.js)). **It has no collision.**
- **Three.js + `camera-controls`** 3.1.2:
  - `minDistance` and `maxDistance`.
  - Wheel or two-finger pinch dollies.
  - `colliderMeshes`, tested with 4 raycasts from the near-plane corners ([readme](https://github.com/yomotsu/camera-controls/blob/dev/readme.md)). The source sets `radius = Math.min(radius, _collisionTest())`, which is a **spring-arm pull-in**: the camera moves in front of the wall instead of stopping (`dist/camera-controls.module.js`, inspected locally).
  - `setFocalOffset(x, y, z)` for the over-the-shoulder offset.
- **angular-three**: `NgtsCameraControls` wraps `camera-controls`; `NgtsOrbitControls` is also available ([soba controls README](https://github.com/angular-threejs/angular-three/blob/main/libs/soba/controls/README.md)).
- **Babylon.js `ArcRotateCamera`**:
  - `lowerRadiusLimit` and `upperRadiusLimit`.
  - Pinch is built in (`pinchPrecision`, `useNaturalPinchZoom`) ([arcRotateCamera.pure.ts](https://github.com/BabylonJS/Babylon.js/blob/master/packages/dev/core/src/Cameras/arcRotateCamera.pure.ts), [arcRotateCameraPointersInput.ts](https://github.com/BabylonJS/Babylon.js/blob/master/packages/dev/core/src/Cameras/Inputs/arcRotateCameraPointersInput.ts)).
  - `targetScreenOffset` for the shoulder offset.
  - **Collision blocks instead of pulling in**: "instead of sliding along obstacles, this camera won't move when a collision happens" ([camera collisions docs](https://doc.babylonjs.com/features/featuresDeepDive/cameras/camera_collisions)). A shoulder camera that gets closer when a wall is in the way needs your own ray cast from the target to the camera, which then shortens `radius`.

### 6. Horizontal FOV (110° horizontal, vertical derived from aspect, clamped in portrait)

- **Three.js**: `PerspectiveCamera.fov` is "the vertical field of view" ([PerspectiveCamera.js r187](https://github.com/mrdoob/three.js/blob/r187/src/cameras/PerspectiveCamera.js)). There is no horizontal mode, so on resize: `fov = clamp(2·atan(tan(55°)/aspect))`, then `updateProjectionMatrix()`. About 3 lines.
- **Babylon.js**: `camera.fovMode = Camera.FOVMODE_HORIZONTAL_FIXED` is built in ([camera.pure.ts](https://github.com/BabylonJS/Babylon.js/blob/master/packages/dev/core/src/Cameras/camera.pure.ts)). **The portrait clamp is custom code anyway.** At 110° horizontal, the vertical FOV is about 78° at 16:9 but about 141° on a 1:2 portrait phone (2·atan(tan 55° / 0.5)). The clamp means switching back to a vertical-fixed FOV below some aspect ratio.
- **angular-three**: same as Three.js.

### 7. Bundle size

Measured locally on 2026-11-22: esbuild 0.28.2, minified ESM, `gzip -9`. Each entry is a minimal scene with renderer, perspective camera with orbit controls, ground and box, lights, ground picking, a glTF load with one animation playing, and a render loop.

| Stack | Initial JS (min) | Initial JS (gzip) | Notes |
|---|---|---|---|
| three r187 + `GLTFLoader` + `camera-controls` 3.1.2 | 684 KB | **170 KB** | one chunk |
| Babylon 9.37.1, deep imports (`Engines/engine`, `scene`, `ArcRotateCamera`, builders, `StandardMaterial`, `glTF/2.0` loader, collisions, picking), code splitting on | 1,326 KB | **321 KB** | plus 302 lazy chunks, 1,043 KB gzip in total (shaders, glTF extensions load on demand); 936 KB gzip as one chunk without splitting |
| Babylon 9.37.1, `import * from '@babylonjs/core'` | 5,187 KB | 1,162 KB | for reference |
| recast-navigation 0.44.0 core + three glue (either engine) | 284 KB entry | 64 KB entry + about 218 KB lazy | the default export is the `wasm-compat` build, with the wasm inlined as base64. The separate `.wasm` is 340 KB raw / 131 KB gzip ([package exports](https://registry.npmjs.org/@recast-navigation/wasm)) |
| angular-three | not measured | | needs a forced peer-dependency override on Angular 22; it adds its renderer on top of three |

### 8. Running the render loop outside Angular change detection

- **Zoneless is the default since Angular v21** ([angular.dev/guide/zoneless](https://angular.dev/guide/zoneless)). Change detection runs only on core notifications: `markForCheck`, `setInput`, "updating a signal that's read in a template", and bound template or host listeners. A `requestAnimationFrame` loop therefore does not trigger change detection, and `runOutsideAngular` is unnecessary in this app. It stays harmless.
- **Three.js**: `renderer.setAnimationLoop(cb)`, which the source recommends over manual rAF ([WebGLRenderer.js r187](https://github.com/mrdoob/three.js/blob/r187/src/renderers/WebGLRenderer.js)). Attach the canvas listeners with `addEventListener`, not template `(pointermove)` bindings, so pointer moves don't schedule change detection. Write a signal only when HUD-visible state changes (for example "arrived at a Setup"), never per frame.
- **Babylon.js**: `engine.runRenderLoop(cb)`. Same rules.
- **angular-three**: runs its own rAF loop ([loop.ts](https://github.com/angular-threejs/angular-three/blob/main/libs/core/src/lib/loop.ts)) with per-frame `beforeRender()` callbacks ([v4 post](https://angularthree.org/blog/v4/)). The scene graph lives in Angular templates through a custom renderer, so binding per-frame values to signals would put change detection on the hot path. It needs the same discipline.

## Recommendation

**Use raw Three.js (r187) + `camera-controls` + `@recast-navigation/three`, hosted in one standalone Angular component** that owns the canvas and runs `setAnimationLoop`. Expose only HUD-relevant state as signals.

Why:

- **No Angular coupling.** `three`, `camera-controls` and `recast-navigation` declare no Angular peer dependency, so Angular 22 today and 23 later need no wait. angular-three is ruled out: it blocks Angular 22 and caps `three` below r183. Revisit it if a release with Angular 22 or later support ships.
- **Camera spring arm out of the box.** `camera-controls` gives min/max distance, pinch, a pull-in camera collision (`colliderMeshes`) and a shoulder offset (`setFocalOffset`). That covers the hardest camera requirement. In Babylon you would write that part yourself.
- **Runtime navmesh from primitive meshes** with `threeToSoloNavMesh`. There is no Blender step, and a tile cache is ready if Setup placement becomes free-form.
- **Smallest initial bundle**: about 170 KB gzip, against about 321 KB for Babylon's deep-import build. Recast adds about 280 KB gzip to either engine.

**Trade-off.** We hand-write glue that Babylon provides built in:
- tap-vs-drag detection (Babylon has `POINTERTAP`)
- the horizontal-FOV formula (Babylon has `FOVMODE_HORIZONTAL_FIXED`, though the portrait clamp is custom in both)
- disposal of geometries, materials and textures on component destroy
- no declarative scene templates (that was angular-three's appeal)
- no built-in inspector

Babylon.js is the runner-up: it is equally decoupled from Angular, more batteries-included, and has a first-party navmesh plugin. It loses on bundle size, on its collision model (it blocks rather than pulls in), and on its navigation plugin fetching recast from unpkg by default.

## Open points for the camera prototype

- Check that `camera-controls` one-finger rotate does not fight a tap-to-move gesture. That means a tap threshold in pixels and milliseconds on `pointerup`.
- Check how a 110° horizontal FOV feels in portrait, and pick the clamp aspect ratio.
- Check whether recast's 64 KB entry plus the lazy wasm chunk should be split off the first paint (dynamic `import()` when the Plot loads).
