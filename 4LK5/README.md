# Response to Reviewer 4LK5

**W1 & Q1 — WorldCast's state representation and update vs. an adapted MultiGen.**
WorldCast takes no global representation of the world as input. Each choice against its MultiGen-style counterpart, on our backbone, data and budget:
- *Update.* MultiGen integrates learned pose increments, so "small pose errors may accumulate". Each WorldCast client re-localizes its player from its own video every block (Eq. 4). A MultiGen-style update trained like our state model drifts on the same generated videos: 11.4 vs. 2.6 body widths at 50–60 s; 5% vs. 39% of clients within two body widths at 60 s (page).
- *Player representation.* MultiGen does not specify how visible players enter its generator. Our camera-aligned field renders them at 0.763, vs. 0.549 for late injection and 0.093 for per-player tokens (Table 5).
- *Shared appearance.* MultiGen shares a map and poses; scene properties are "not explicitly preserved". Turning off WorldCast's shared generated blocks lowers all four closed-loop agreement measures (Table 4a) and costs 1.23 dB PSNR after 30 s (Table 18; page videos).

**W1 — One GPU per player is a choice, not a requirement.**
Agreed; networked play favors it. A WorldCast client receives 2.2–3.5 Mb/s; joint generation split one player per GPU needs 149 Mb/s–389 Gb/s (Table 7). Measured between two machines, Solaris's exchange alone exceeds the 62.5-ms frame at every player count; WorldCast's takes under 1 ms per 1-s block (Table R3).

**W2a — FPS excludes waits; latency not evaluated.**
- Deployment reads the latest messages without waiting (App. B); lockstep is evaluation-only. With its waits counted, our optimized serving path (tiny decoder) runs 31.0–39.0 FPS per client for 2–16 clients on H200, vs. 18.7–23.4 in Table 7 (Table R5).
- Key press to screen: 0.73 s at the median (p95 1.18 s), set by the 1-s block as in any block-wise generator; the first frame follows the control read in under 0.2 s (page figure).

**W2b — Network delays not evaluated.**
Closed loop, paired vs. on-time messages: Rendered changes by −0.03 with random 0–2-block delays, 0.00 / −0.01 with 10% / 30% of messages lost, 0.00 with one client always a block late, −0.07 with every message a block late; image quality is unchanged.

**W2c — Hour-long runs: one client, recorded states for others, resets.**
New closed-loop runs lift all three, nothing reset: all ten players for one hour (VBench imaging 71.1) and five clients for three hours (71.5 / 72.7 / 73.1 in hours 1–3; page tables and videos).
With recorded positions, half a Dust2 match (13 min, 12 rounds, five clients, scene state never cleared) scores Rendered 0.836, Unmatched 0.045 (per minute on the page).

**W2d — Firing missing in other views; why not evaluated?**
- Firing is modeled: the field carries each visible player's attack and reload keys and weapon (App. F). Page videos (attack held / attack zeroed / recording): the flash follows the attack channel.
- Weapons and aim are drawn; the flash is less frequent, as a close, visible firing player appears in only 0.5% of training frames.
- No detector reads firing or weapon reliably, even on recordings; readable attributes match near recording level: facing 0.825 vs. 0.868; with the state model, airborne 0.85 vs. 0.89.

**Q2 — Four-step examples, evidence against the teacher, longest closed loop.**
- All supplementary videos except Section 01 (Table 1's bidirectional comparison) are four-step.
- Same windows and inputs, four-step minus teacher (Table R13): Rendered +0.033, Unmatched 0.000, VBench imaging +1.7, PSNR −0.14 dB, LPIPS +0.005; side-by-side videos on three maps. The four-step model also draws sharper player contours (1.12×).
- Longest closed loop: Section 02c (five clients, 54 s) and the three-hour run (W2c).

**Q3 — Consistency with more players than in training.**
- Each training sample is one player's window, and the field's size does not grow with the player count.
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
- [W2c — Hour-long runs: one client, recorded states for others, resets](index.html#w2c): 13 minutes per minute; image quality over hours; videos
- [W2d — Firing missing in other views; why not evaluated?](index.html#w2d): attack held, attack zeroed, recording
- [Q2 — Four-step examples, evidence against the teacher, longest closed loop](index.html#q2): four-step model vs. its teacher
- [Q3 — Consistency with more players than in training](index.html#q3): more clients; a world of twenty
