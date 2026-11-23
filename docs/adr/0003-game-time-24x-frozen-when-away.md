# Game time runs 24× faster than the wall clock and stops while the game is not on screen

`now` counts game milliseconds, and `game.tick(dtMs)` adds `dtMs × 24`, so a game day lasts one real hour: the Plot gets days, Opening hours and weeks within a play session. Nothing progresses while the tab is hidden or the game is closed: `tick` caps `dtMs`, and the save holds only `now`, never the wall clock. Many management games catch up on return instead. We rejected that because it needs the wall clock in the save, a returning player would find Patients piled up in the Consultation room, and changing the system clock would cheat it. Sleeping on the Couch is the only way to skip time; there is no pause and no speed control.

## Consequences

- Content durations (growing, Batches, delivery) are written in game time.
- Dates come from a fixed start, 2027-01-01 08:00, through `Date` in UTC, so time zones and daylight saving never apply.
- The large `dtMs` after a hidden tab is capped, not caught up in one `ADVANCE` as [ADR 0002](0002-framework-free-game-core.md) allowed. Sleep uses that same one-call `ADVANCE` to jump `now` forward.
- Waiting must never cost the player anything, since they can't pause: plants don't rot, Batches can't fail, and Patients don't leave because they waited.

Decided in: [How does game time pass?](https://github.com/clemeno/calchimi/issues/10)
