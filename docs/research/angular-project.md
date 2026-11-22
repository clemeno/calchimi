# What a new Angular + Angular Material 3 project looks like (2026-11-22)

Research for [#4](https://github.com/clemeno/calchimi/issues/4), feeding [#7](https://github.com/clemeno/calchimi/issues/7) (pick the `calchfront` stack).

Method: facts come from the published npm packages (`npm view`, and the `@schematics/angular@22.3.0`, `@angular/material@22.3.0` and `@angular-devkit/schematics@22.3.0` tarballs, read but not run), the matching source at the `v22.3.0` git tags, the official changelogs, angular.dev and the Angular Material guides. Every claim cites its source. Source keys like [S1] point to the list at the end.

## TL;DR

- Current stable: **Angular 22.3.0** (CLI, framework, Material and CDK all at 22.3.0, released 2026-11-18/19). v23 is not due until about **June 2027**, because Angular moved to one major release a year after v22. [S1][S2][S3]
- `ng new` defaults: **standalone**, **zoneless** (no zone.js), components **OnPush by default**, **esbuild** `@angular/build:application` builder with a **Vite**-based dev server, **Vitest + jsdom** tests, **TypeScript 6.0**, Prettier, no linter. It asks about the **stylesheet format (CSS by default), SSR (No by default) and AI tool config (None by default)**. It does not ask about zoneless. [S4][S5][S6][S7][S8][S9]
- `ng add @angular/material` asks only for a palette pair. It writes a `mat.theme()` Sass block that emits Material 3 `--mat-sys-*` CSS variables built with `light-dark()`. Dark mode is just `color-scheme` (`light`, `dark` or `light dark`). Custom palettes come from `ng generate @angular/material:theme-color`. [S10][S11][S12][S13]
- Package managers: **npm, yarn, pnpm, bun**. As of 2026-11-22 there are no open angular-cli issues with "bun" or "pnpm" in the title. Both got a run of fixes during 22.x. pnpm projects now get a generated `pnpm-workspace.yaml` with `allowBuilds`. [S4][S14][S15][S16]
- v21 → v22 breaking changes that touch a fresh project: **OnPush is the default change detection**, **TypeScript ≥ 6.0 <6.1**, **Node 22.22+/24.15+/26+** (Node 20 dropped), the **Hammer.js integration was removed**, and the router now defaults to `paramsInheritanceStrategy: 'always'`. [S3][S17][S18][S19]

## 1. Current stable version

| Package | `latest` on npm | Released |
|---|---|---|
| `@angular/cli` | 22.3.0 | 2026-11-19 (npm time) |
| `@angular/core` | 22.3.0 | 2026-11-18 (changelog) |
| `@angular/material`, `@angular/cdk` | 22.3.0 | with core |

- `npm view @angular/cli dist-tags` shows `latest: 22.3.0` and `next: 22.3.0-rc.0`. No `23.0.0-*` prerelease exists yet. [S1]
- The release page says: "A major release every 12 months" and "Until Angular v22, Angular had a 6-month major release cycle". v22 is **Active** (released 2026-06-03, active until 2027-06, LTS until 2028-06). v21 is in LTS until 2027-06. "v23.0 | ~ June 2027". [S2]
- Version compatibility for 22.x: Node `^22.22.3 || ^24.15.0 || ^26.0.0`, TypeScript `>=6.0.0 <6.1.0`, RxJS `^6.5.3 || ^7.4.0`. Browser support is Baseline "widely available" as of 2026-05-07. [S20] The published `engines` field matches this. [S1]
- `@angular/material@22.3.0` already declares peer support for `@angular/core ^22.0.0 || ^23.0.0`. [S1]

## 2. What `ng new` generates by default (CLI 22.3.0)

| Aspect | Default | Source |
|---|---|---|
| Standalone (no NgModules) | `standalone: true` | ng-new schema [S4] |
| Zoneless | `zoneless: true` in the application schematic. `app.config.ts` only adds `provideZoneChangeDetection` when zoneless is off, so a default app has no zone.js and needs no zoneless provider. | application schema [S5], `app.config.ts.template` [S6]. "Zoneless is the default in Angular v21+" [S21] |
| Change detection | Components with no `changeDetection` are **OnPush** (framework default since v22). `ng g component` also defaults to `OnPush` (choices: `Eager`, `OnPush`). | core changelog [S3], component schema [S22] |
| Root providers | `provideBrowserGlobalErrorListeners()`, plus `provideRouter(routes)` (routing is on by default) | [S6], [S5] |
| Build | `@angular/build:application` (esbuild). Dev server: `@angular/build:dev-server`. | workspace-models [S7], angular.dev build [S23] |
| Dev server engine | Vite (`@angular/build@22.3.0` depends on `vite@8.3.8` and `esbuild@0.28.2`) | npm deps [S1] |
| Unit tests | `testRunner: "vitest"` with `@angular/build:unit-test`. Dependencies added: `vitest` (`^5.0.0`) and `jsdom` (`^31.0.0`). Karma is still available via `--test-runner=karma`, but that option was **deprecated in 22.3.0**: "New projects will only be configured with Vitest in a future version." | ng-new schema [S4], dependencies.ts [S8], latest-versions [S9], CLI changelog 22.3.0 [S17], angular.dev testing [S24] |
| Browser-mode tests | Opt in: the `browsers` option plus a Playwright or WebdriverIO provider | [S24] |
| Styles | `style` prompt, default `css`. Options: `css`, `tailwind`, `scss`, `sass`, `less`. | application schema [S5] |
| SSR | Prompt: "Do you want to enable Server-Side Rendering (SSR) and Static Site Generation (SSG/Prerendering)?" Default `false`. | application schema [S5] |
| AI config | Prompt: "Which AI tools should Angular integrate with?" Default `none`. `claude-code` writes `CLAUDE.md` and `.mcp.json` (Angular MCP server). | ai-config schema [S25] |
| File names | `fileNameStyleGuide: "2025"`: `app.ts` / `app.html`, not `app.component.ts` | ng-new schema [S4], app template [S6] |
| TypeScript | `~6.0.2` | latest-versions [S9] |
| Formatting / lint | `prettier` devDependency plus `.prettierrc` (printWidth 100, singleQuote, `angular` parser for HTML). **No lint target.** | package.json template [S26], `.prettierrc.template` [S26] |
| Strict mode | `strict: true` (strict TypeScript and tighter budgets) | ng-new schema [S4] |
| Git | Initialises git and commits. When the target is **already inside a git work tree**, it logs "Directory is already under version control. Skipping initialization of git." | repo-init executor [S27] |

Prompts in an interactive `ng new` (from the `x-prompt` entries): project name, stylesheet, SSR, AI tools. `--defaults` takes the defaults without asking. [S4][S5][S25]

## 3. Angular Material and Material Design 3 theming

**Setup with `ng add @angular/material`** [S10][S12]
- One prompt: the starter palette pair, default `azure-blue`. The others are `rose-red`, `magenta-violet` and `cyan-orange`.
- It inserts a theme at the top of the project's `.scss` global stylesheet. If the project uses plain CSS, it creates `src/material-theme.scss` and adds it to the build `styles` instead. This is why `--style=scss` is the simpler choice. [S12]
- It adds `<link>` tags for Roboto (300/400/500) and **Material Symbols Outlined** to `index.html`. [S28]
- No animations setup is needed. Material 22.3.0's peer deps do not include `@angular/animations` [S1], and that package has been deprecated since 20.2.0. [S3]

**Generated theme** (from `create-theme.ts`, abridged) [S11]

```scss
@use '@angular/material' as mat;
html {
  height: 100%;
  @include mat.theme((
    color: (primary: mat.$azure-palette, tertiary: mat.$blue-palette),
    typography: Roboto,
    density: 0,
  ));
}
body {
  color-scheme: light;               // 'dark' or 'light dark' to follow the OS
  background-color: var(--mat-sys-surface);
  color: var(--mat-sys-on-surface);
  font: var(--mat-sys-body-medium);
  margin: 0;
  height: 100%;
}
```

**How M3 theming works** [S13]
- "The `mat.theme` mixin takes a map that defines color, typography, and density values and outputs a set of CSS variables". These are the Material 3 system tokens (`--mat-sys-surface`, `--mat-sys-primary-container`, `--mat-sys-body-large`, …). The mixin emits only the categories you pass in.
- Your own components use those variables directly (`background: var(--mat-sys-primary-container)`). `mat.system-classes()` adds utility classes such as `mat-bg-surface` and `mat-text-on-surface`. This mixin is exported by the 22.3.0 package. [S13][S1]
- **Dark mode**: color tokens are defined with CSS `light-dark()`, so the mode follows `color-scheme`. Use `light`, `dark`, `light dark` (follow the system), or a class such as `body.dark-mode { color-scheme: dark; }`. "If your application does not define a value for `color-scheme`, then the light colors will always be applied." Material does not apply `prefers-color-scheme` or `prefers-contrast` on its own.
- **Palettes**: there are 12 prebuilt palettes (`$red-palette` … `$rose-palette`). For custom ones, `ng generate @angular/material:theme-color` (alias of `m3Theme`) builds palettes from a primary color, plus optional secondary, tertiary, neutral, neutral-variant and error colors. It can also emit high-contrast override mixins (`includeHighContrast`) and write SCSS or CSS (`isScss`). [S13][S29]
- **Customizing**: use `mat.theme-overrides((primary-container: …))` or the `$overrides` argument of `mat.theme` for system tokens, and per-component `mat.<component>-overrides(...)` (for example `mat.card-overrides`) for component tokens. Angular "strongly discourages" overriding component CSS outside these APIs. [S13]
- **No Sass at all?** There are 4 prebuilt M3 CSS themes (`azure-blue.css`, `rose-red.css`, `cyan-orange.css`, `magenta-violet.css`) to drop into `angular.json` `styles`. The custom `mat.theme` route needs a Sass theme file. [S13]

## 4. Package managers

- `ng new --package-manager` accepts `npm`, `yarn`, `pnpm`, `bun`. [S4] angular.dev documents a global CLI install for each one. [S30]
- **npm**: the baseline. I found no open npm-specific issue.
- **pnpm**: since 22.2.6, `ng new --package-manager=pnpm` writes a `pnpm-workspace.yaml` with `packages: ['.']` and `allowBuilds` entries (`@parcel/watcher`, `esbuild`, `lmdb`, `msgpackr-extract`, all `false`), so pnpm's build-script gate does not block or warn. [S16][S14] Other recent fixes: temp installs isolated from a parent pnpm workspace (22.0.1), subproject dependency resolution in pnpm workspaces (22.0.7), and `pnpm-workspace.yaml` sanitization with pnpm patches (22.2.7). [S14] No open issue has "pnpm" in its title. [S15]
- **bun**: supported since CLI 17.2.0 (`add support to bun package manager`). [S14] 22.x fixes: registry metadata fetching under bun (22.0.2), registry error and 404 handling, and subproject resolution in bun workspaces (22.2.5). [S14] No open issue has "bun" in its title. [S15]
- **yarn**: old open issues remain: `ng update` with yarn workspaces (#14841) and Yarn PnP asset paths (#25263). [S15]
- **Applies to every manager**: open regression angular/angular-cli#34688. Since 22.3.0-next.5, `ng update`'s final reinstall skips lifecycle scripts (`--ignore-scripts`) without telling you. This bites `patch-package` and binary-downloading postinstalls. It does not affect `ng new`. [S31]
- **Not supported**: Deno (open feature request #30947). [S15]
- **Windows**: a global CLI install from PowerShell may need `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`. [S30]
- This machine (checked 2026-11-22): Node v26.13.1 (inside the supported `^26`), npm 11.9.0, bun 1.4.5, pnpm not installed.

## 5. Breaking changes since v21 that affect a fresh project

From the 22.0.0 changelogs (2026-06-03). Upgrade-only items are left out. [S3][S18][S19]
- **OnPush by default**: "Component with undefined `changeDetection` property are now `OnPush` by default." Opt out per component with `ChangeDetectionStrategy.Eager`. With zoneless, views refresh on signal writes read by templates, bound event listeners, `markForCheck`/`AsyncPipe` and `setInput`. Plain timers and callbacks do not trigger a refresh. [S3][S21]
- **TypeScript < 6.0 dropped**. The compiler now reports `nullishCoalescingNotNullable` / `optionalChainNotNullable` diagnostics, and templates using `in` variables throw. [S3]
- **Node 20 dropped**. The CLI changelog gives the minimums as 22.22.0 / 24.13.1 [S18], and the 22.3.0 `engines` field requires `^22.22.3 || ^24.15.0 || >=26.0.0`. [S1]
- **Hammer.js integration removed** from platform-browser: "Use your own implementation." [S3]
- **Router**: `paramsInheritanceStrategy` now defaults to `'always'`, and `provideRoutes()` was removed. [S3]
- **Forms**: `min`/`max` validators accept only numbers or null. [S3]
- **HTTP**: `withFetch` is deprecated because fetch is now the default backend. Use `provideHttpClient(withXhr)` if you need upload progress. [S3]
- **Dev server**: the `PORT` env var now overrides `angular.json` and `--port`. [S18]
- **Build**: the webpack builders in `@angular-devkit/build-angular` are deprecated in favor of `@angular/build`. The Jest and Web Test Runner builders were removed. [S18]
- **SSR**: requests with an unrecognized `Host` now return 400 instead of falling back to CSR, and `CommonEngine` is deprecated. [S18]
- **Material/CDK 22**: small API removals and renames (`MatListOption.checkboxPosition` → `togglePosition`, CDK a11y constants, aria combobox rename). Nothing a fresh app uses on day one. [S19]

## What this means for `calchfront` (inference, not sourced)

- SSR: answer **No**. The game uses WebGL and localStorage, which only exist in the browser.
- Zoneless + OnPush suits a 3D render loop. A `requestAnimationFrame` loop does not trigger Angular change detection by itself, so the HUD and Material panels update only when game state is written to signals. Keep per-frame scene data out of signals.
- Touch input (tap, hold-orbit, pinch-zoom) should use Pointer Events on the canvas. Hammer.js is no longer an option. [S3]
- `ng new` inside the existing `calchimi` repo skips `git init`. [S27]

## Recommended `ng new` flags

`ng new calchfront --style=scss --ssr=false --package-manager=npm --ai-config=none` (zoneless, standalone, routing and Vitest are already the defaults. Use `--package-manager=bun` if #7 picks bun. Use `--ai-config=claude-code` to get an Angular `CLAUDE.md` and MCP config in `calchfront/`.)

## Sources

- [S1] npm registry, queried 2026-11-22: `npm view @angular/cli version dist-tags time`, `npm view @angular/core|@angular/material|@angular/cdk|@angular/build version peerDependencies engines dependencies`. https://www.npmjs.com/package/@angular/cli
- [S2] Angular versioning and releases: https://angular.dev/reference/releases
- [S3] angular/angular CHANGELOG, 22.0.0 (Breaking Changes, Deprecations) and 20.2.0 (`deprecate the animations package`): https://github.com/angular/angular/blob/v22.3.0/CHANGELOG.md
- [S4] `ng-new` schema: https://github.com/angular/angular-cli/blob/v22.3.0/packages/schematics/angular/ng-new/schema.json
- [S5] `application` schema: https://github.com/angular/angular-cli/blob/v22.3.0/packages/schematics/angular/application/schema.json
- [S6] Application templates: https://github.com/angular/angular-cli/tree/v22.3.0/packages/schematics/angular/application/files/standalone-files/src
- [S7] Builder names: https://github.com/angular/angular-cli/blob/v22.3.0/packages/schematics/angular/utility/workspace-models.ts
- [S8] Test dependencies: https://github.com/angular/angular-cli/blob/v22.3.0/packages/schematics/angular/utility/dependencies.ts
- [S9] Pinned versions: https://github.com/angular/angular-cli/blob/v22.3.0/packages/schematics/angular/utility/latest-versions/package.json
- [S10] Material `ng-add` schema: https://github.com/angular/components/blob/v22.3.0/src/material/schematics/ng-add/schema.json
- [S11] Generated theme: https://github.com/angular/components/blob/v22.3.0/src/material/schematics/ng-add/theming/create-theme.ts
- [S12] Theme insertion logic: https://github.com/angular/components/blob/v22.3.0/src/material/schematics/ng-add/theming/theming.ts
- [S13] Material theming guide (identical on v22.3.0 and main): https://github.com/angular/components/blob/v22.3.0/guides/theming.md (rendered: https://material.angular.dev/guide/theming)
- [S14] angular/angular-cli CHANGELOG (22.0.1, 22.0.2, 22.0.7, 22.2.5, 22.2.6, 22.2.7 entries, bun support entries): https://github.com/angular/angular-cli/blob/main/CHANGELOG.md
- [S15] Open angular-cli issues, 2026-11-22: `gh search issues --repo angular/angular-cli --state open "bun in:title"` / `"pnpm in:title"` (none) / `"yarn in:title"` (#14841, #25263). Deno: https://github.com/angular/angular-cli/issues/30947
- [S16] pnpm template: https://github.com/angular/angular-cli/blob/v22.3.0/packages/schematics/angular/workspace/files/pnpm-workspace.yaml.template
- [S17] angular-cli CHANGELOG 22.3.0 (testRunner deprecation): https://github.com/angular/angular-cli/blob/main/CHANGELOG.md#2230-2026-11-18
- [S18] angular-cli CHANGELOG 22.0.0 (Breaking Changes, Deprecations): https://github.com/angular/angular-cli/blob/main/CHANGELOG.md
- [S19] angular/components CHANGELOG 22.0.0: https://github.com/angular/components/blob/main/CHANGELOG.md
- [S20] Version compatibility: https://angular.dev/reference/versions
- [S21] Zoneless guide: https://angular.dev/guide/zoneless
- [S22] Component schema: https://github.com/angular/angular-cli/blob/v22.3.0/packages/schematics/angular/component/schema.json
- [S23] Build guide: https://angular.dev/tools/cli/build. Serve guide: https://angular.dev/tools/cli/serve
- [S24] Testing guide: https://angular.dev/guide/testing
- [S25] AI config schema: https://github.com/angular/angular-cli/blob/v22.3.0/packages/schematics/angular/ai-config/schema.json
- [S26] Workspace templates (`package.json.template`, `.prettierrc.template`): https://github.com/angular/angular-cli/tree/v22.3.0/packages/schematics/angular/workspace/files
- [S27] Git init task: `@angular-devkit/schematics@22.3.0` `tasks/repo-init/executor.js` (source: https://github.com/angular/angular-cli/blob/v22.3.0/packages/angular_devkit/schematics/tasks/repo-init/executor.ts)
- [S28] Font links: https://github.com/angular/components/blob/v22.3.0/src/material/schematics/ng-add/fonts/material-fonts.ts
- [S29] `theme-color` schema: https://github.com/angular/components/blob/v22.3.0/src/material/schematics/ng-generate/theme-color/schema.json
- [S30] Local setup: https://angular.dev/tools/cli/setup-local
- [S31] https://github.com/angular/angular-cli/issues/34688
