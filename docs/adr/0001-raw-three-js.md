# Raw Three.js in one Angular component, not angular-three or Babylon.js

`calchfront` renders the Plot with raw Three.js, `camera-controls` and `@recast-navigation/three`, all hosted in one standalone Angular component that owns the canvas and the `setAnimationLoop`. The Angular wrapper angular-three would seem the obvious choice, but it caps `@angular/core` below 22 and `three` below r183, and no maintainer has answered the request for Angular 22 support. Babylon.js 9 was the runner-up: it has more built in, but its initial bundle is about 321 KB gzip against about 170 KB for Three.js, and its camera stops at walls instead of moving in front of them, which a shoulder camera needs. Babylon's navigation plugin also fetches recast from unpkg by default.

## Consequences

- We write what Babylon would have given us: telling a tap from a drag, the horizontal-FOV formula, and disposing geometries, materials and textures when the component is destroyed.
- The scene is not declared in Angular templates. Angular sees only HUD-relevant state, written to signals when it changes, never every frame.
- Revisit angular-three if a release supports the Angular version in use.

Research: [Which 3D library fits a zoneless Angular app for third-person click-to-move play?](https://github.com/clemeno/calchimi/issues/3)
