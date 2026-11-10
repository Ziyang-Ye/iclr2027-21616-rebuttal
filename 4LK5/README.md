# Response to Reviewer 4LK5

**W1 & Q1 — WorldCast's state representation and update vs. an adapted MultiGen.**
We agree MultiGen shares this outline and will say so. The difference is the state itself: MultiGen's is a given, editable map with simulated poses; WorldCast's is observed from each client's generated video, so a player's own view and what others draw stay coupled, and clients share generated appearance, with no given map. With backbone and domain fixed, choice by choice:
- *Update.* MultiGen integrates learned pose increments, so "small pose errors may accumulate". Each WorldCast client re-localizes its player from its own video every block (Eq. 4). MultiGen-style updates with a map ray-cast drift on videos generated with recorded positions: 7.4–11.4 vs. 2.6 body widths at 50–60 s; in closed loop the state's height is a median 0.26 body widths from the player's frames, vs. 0.65–0.73 (Table R1b).
- *Player representation.* MultiGen ray-casts the map; how players enter is unspecified. Same backbone and data: camera-aligned field 0.763, late injection 0.549, per-player tokens 0.093 Rendered (Table 5).
- *Shared appearance.* MultiGen shares a map and poses; scene properties are "not explicitly preserved". Turning off WorldCast's shared generated blocks lowers all four closed-loop agreement measures (Table 4a) and costs 1.23 dB PSNR after 30 s (Table 18; page videos).

**W1 — One GPU per player is a choice, not a requirement.**
Agreed: it is a deployment choice; our 15-minute run places two clients on one GPU. A WorldCast client receives 2.2–3.5 Mb/s; joint generation split one player per GPU needs 149 Mb/s–389 Gb/s (Table 7). Between two machines, the exchange for one second of video takes 1.37–11.0 s for Solaris and 0.40–0.49 ms for WorldCast (Table R3).

**W2a — FPS excludes waits; latency not evaluated.**
- Deployment reads the latest messages without waiting (App. B); lockstep is evaluation-only. With waits counted and Table 7's full decoder, engineering optimizations bring the serving path to 24.1–28.2 FPS for 2–16 clients on H200 (Table 7, no waits: 18.7–23.4); with a tiny decoder, 30.7–38.9 (Table R5).
- Key press to screen: 0.73 s median (p95 1.18 s) with the tiny decoder, 0.94 / 1.40 s with Table 7's (Table R6).

**W2b — Network delays not evaluated.**
Closed loop, vs. on-time messages: Rendered changes by −0.03 with random 0–2-block delays, 0.00 / −0.01 with 10% / 30% of messages lost, 0.00 with one client always a block late, −0.07 with every message a block late; image quality is unchanged.

**W2c — Hour-long runs.**
New closed-loop runs: recorded actions only, state-model positions, nothing reset. One hour, five clients (Table R9c): Rendered of clearly visible players is 0.800, 0.667, 0.764 per 10 min in the first 30 min; later, and with ten clients, agreement falls. VBench imaging: 71.5 (ten: 71.1). 15 min on one track (Table R9): 0.643 vs. 0.800 with recorded positions.
With recorded positions, a 13-min Dust2 half (12 rounds, five clients, scene state kept) scores Rendered 0.836, Unmatched 0.045.

**W2d — Firing missing in other views; why not evaluated?**
- Hits and deaths are out of scope by design. Firing is modeled: the field carries each visible player's attack and reload keys and weapon (App. F). Page videos (attack held / attack zeroed / recording): the flash follows the attack channel.
- Missing flashes reflect data: a close, visible firing player appears in only 0.5% of training frames.
- Firing and weapon readers are unreliable even on recordings, so these are shown, not scored.

**Q2 — Four-step examples, evidence against the teacher, longest closed loop.**
- All supplementary videos except Section 01 (Table 1's bidirectional comparison) are four-step.
- Same windows and inputs, four-step minus teacher (Table R12): Rendered +0.033, Unmatched 0.000, VBench imaging +1.7, PSNR −0.14 dB, LPIPS +0.005; side-by-side videos on four maps.
- Longest closed loop: the one-hour runs (W2c).

**Q3 — Consistency with more players than in training.**
- The field's size does not grow with the player count.
- A twenty-player world, twice any training match (recorded positions): the paired Rendered difference from the ten-player reference is −0.013, and Unmatched is unchanged (Table R14, page video).
- With ten players as closed-loop clients, Rendered is 0.814; the up-to-seven-client reference is 0.821.

**Minor — Fig. 2.** Agreed; the revision simplifies it.

**Revisions:** abstract, introduction and conclusion will state the evaluation protocols and one GPU per player as a deployment choice; Table 7 will add rates with waits; Related Work will expand the MultiGen comparison.

Anonymous page (figures, tables, videos): [index.html](index.html)

## Figures, tables and videos

- [W1 & Q1 — WorldCast's state representation and update vs. an adapted MultiGen.](index.html#w1q1): position error over a minute, tracks, scene state
- [W1 — One GPU per player is a choice, not a requirement](index.html#w1): exchange time between two machines
- [W2a — FPS excludes waits; latency not evaluated](index.html#w2a): speed and latency
- [W2b — Network delays not evaluated](index.html#w2b): delayed and lost messages
- [W2c — Hour-long runs](index.html#w2c): 13 minutes, minute by minute; image quality over one hour; videos
- [W2d — Firing missing in other views; why not evaluated?](index.html#w2d): attack held, attack zeroed, recording
- [Q2 — Four-step examples, evidence against the teacher, longest closed loop](index.html#q2): four-step model vs. its teacher
- [Q3 — Consistency with more players than in training](index.html#q3): more clients; a world of twenty
