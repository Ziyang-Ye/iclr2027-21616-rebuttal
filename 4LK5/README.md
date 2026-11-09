# Response to Reviewer 4LK5

**W1 & Q1 — WorldCast's state representation and update vs. an adapted MultiGen.**
The main advantage: the shared state is observed from the video each client generated, not simulated, so a player's own view and what others draw stay coupled; clients also share generated appearance, with no given map. MultiGen follows a given, editable map; WorldCast targets consistent views and player states in scenes learned in training. Inputs and goals differ; we compare each choice within our stack:
- *Update.* MultiGen integrates learned pose increments, so "small pose errors may accumulate". Each WorldCast client re-localizes its player from its own video every block (Eq. 4). A MultiGen-style update trained like our state model drifts on the same generated videos: 11.4 vs. 2.6 body widths at 50–60 s.
- *Player representation.* MultiGen does not specify how visible players enter its generator. Our camera-aligned field renders them at 0.763, vs. 0.549 for late injection and 0.093 for per-player tokens (Table 5).
- *Shared appearance.* MultiGen shares a map and poses; scene properties are "not explicitly preserved". Turning off WorldCast's shared generated blocks lowers all four closed-loop agreement measures (Table 4a) and costs 1.23 dB PSNR after 30 s (Table 18; page videos).

**W1 — One GPU per player is a choice, not a requirement.**
Agreed: it is a deployment choice; our 13-minute run places two clients on one GPU. A WorldCast client receives 2.2–3.5 Mb/s; joint generation split one player per GPU needs 149 Mb/s–389 Gb/s (Table 7). Between two machines, Solaris's exchange alone exceeds the 62.5-ms frame; WorldCast's takes under 1 ms per 1-s block (Table R3).

**W2a — FPS excludes waits; latency not evaluated.**
- Deployment reads the latest messages without waiting (App. B); lockstep is evaluation-only. With waits counted and Table 7's full decoder, engineering optimizations bring the serving path to 24.1–28.2 FPS for 2–16 clients on H200 (Table 7, no waits: 18.7–23.4); with a tiny decoder, 30.7–39.0 (Table R5).
- Key press to screen: 0.73 s median (p95 1.18 s) with the tiny decoder, 0.94 / 1.40 s with Table 7's; A's generated motion reaches B's screen in 1.74 s (page).

**W2b — Network delays not evaluated.**
Closed loop, vs. on-time messages: Rendered changes by −0.03 with random 0–2-block delays, 0.00 / −0.01 with 10% / 30% of messages lost, 0.00 with one client always a block late, −0.07 with every message a block late; image quality is unchanged.

**W2c — Hour-long runs.**
New closed-loop runs give each client only its recorded actions; positions come from the state model, nothing is reset: ten clients (VBench imaging 71.1) and five clients (71.5) for one hour (page videos).
With recorded positions, half a Dust2 match (13 min, 12 rounds, five clients, scene state never cleared) scores Rendered 0.836, Unmatched 0.045 (per minute: page).

**W2d — Firing missing in other views; why not evaluated?**
- Hits and deaths are out of scope by design. Firing is modeled: the field carries each visible player's attack and reload keys and weapon (App. F). Page videos (attack held / attack zeroed / recording): the flash follows the attack channel.
- Missing flashes are not a design choice: a close, visible firing player appears in only 0.5% of training frames.
- No detector reads firing or weapon reliably, even on recordings; readable attributes match near recording level: facing 0.825 vs. 0.868; with the state model, airborne 0.85 vs. 0.89.

**Q2 — Four-step examples, evidence against the teacher, longest closed loop.**
- All supplementary videos except Section 01 (Table 1's bidirectional comparison) are four-step.
- Same windows and inputs, four-step minus teacher (Table R13): Rendered +0.033, Unmatched 0.000, VBench imaging +1.7, PSNR −0.14 dB, LPIPS +0.005; side-by-side videos on four maps.
- Longest closed loop: Section 02c (five clients, 54 s) and the one-hour runs (W2c).

**Q3 — Consistency with more players than in training.**
- The field's size does not grow with the player count.
- A twenty-player world, twice any training match (recorded positions): the paired Rendered difference from the ten-player reference is −0.013, and Unmatched is unchanged (Table R15, stills).
- With ten players as closed-loop clients, Rendered is 0.814; the up-to-seven-client reference is 0.821.

**Minor — Fig. 2 is too packed.** Agreed; the revision simplifies it.

**Revisions:** abstract, introduction and conclusion will state the evaluation protocols and one GPU per player as a deployment choice; Table 7 will add rates with waits; Related Work will expand the MultiGen comparison.

Anonymous page (figures, tables, videos): [index.html](index.html)

## Figures, tables and videos

- [W1 & Q1 — WorldCast's state representation and update vs. an adapted MultiGen.](index.html#w1q1): position error over a minute, tracks, scene state
- [W1 — One GPU per player is a choice, not a requirement](index.html#w1): exchange time between two machines
- [W2a — FPS excludes waits; latency not evaluated](index.html#w2a): speed and latency
- [W2b — Network delays not evaluated](index.html#w2b): delayed and lost messages
- [W2c — Hour-long runs](index.html#w2c): 13 minutes per minute; image quality over one hour; videos
- [W2d — Firing missing in other views; why not evaluated?](index.html#w2d): attack held, attack zeroed, recording
- [Q2 — Four-step examples, evidence against the teacher, longest closed loop](index.html#q2): four-step model vs. its teacher
- [Q3 — Consistency with more players than in training](index.html#q3): more clients; a world of twenty
