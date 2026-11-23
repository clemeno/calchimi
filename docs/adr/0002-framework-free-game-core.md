# Game core is a framework-free TypeScript module, held by Angular in one signal

`calchfront` keeps all game state and rules in `src/app/game/`: one plain, JSON-serialisable `GameState` and pure functions that return a new state, importing neither Angular nor Three.js (a lint rule enforces it). A root `Game` service holds that state in one signal, and the 3D component's render loop drives the game clock through `game.tick(dt)` and updates the scene when the state reference changes. Angular services with one signal per field would have needed less glue, but the state would no longer be one object to save and the rules would be tested through Angular; a store library such as NgRx SignalStore adds a dependency for nothing at this size.

## Consequences

- The state records when things happen in game time (`plantedAt`, `endsAt`, `nextPatientAt`), not countdowns: `ADVANCE({ state, now })` returns a new state only when an event is due, so the signal is never written every frame ([ADR 0001](0001-raw-three-js.md)) and catching up after a hidden tab is one call.
- A rule must never mutate its input, or the signal and the scene miss the change: tests deep-freeze the input state.
- Randomness comes from a seed stored in the state, so rules stay pure and reloading a save does not redraw.
- Game time runs only while the play screen renders, since the render loop drives it.

Decided in: [Where does game state live relative to Angular and the 3D scene?](https://github.com/clemeno/calchimi/issues/9)
