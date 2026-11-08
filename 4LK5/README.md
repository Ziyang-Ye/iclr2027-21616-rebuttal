# Response to Reviewer 4LK5

We thank the reviewer. Unless run with the state model, consistency results give clients the recorded positions, as in the paper's main tables. This controlled design, credited by Reviewer cvu7, separates rendering from state estimation and locates each source of error; with dense real-match inputs (flicks, strafing, jumps), action-only rollouts depart from their recordings (App. G). Rates and latency are of the full system. New tables, figures and videos are on the anonymous page.

**W1 & Q1 — What does WorldCast add over MultiGen?**
MultiGen conditions each view on a ray-cast of a given map and advances poses by learned increments. WorldCast takes no global representation of the world as input, which removes the two limits MultiGen's Sec. 6 states:
- *Pose errors "may accumulate over long rollouts".* Each client re-localizes its player from its own generated video every block (Eq. 4). A MultiGen-style update, trained like our state model, drifts: 11.4 vs. 2.6 body widths at 50–60 s; 5% vs. 39% of clients within two body widths at 60 s (page: error curves, tracks).
- *Scene properties "not explicitly preserved".* Clients share their generated blocks instead of a map; turning this off lowers all four closed-loop agreement measures (Table 4a) and costs 1.23 dB PSNR after 30 s (Table 18; page videos).

**W1 — One GPU per player is a choice, not a requirement.**
Agreed; networked play favors it. A WorldCast client receives 2.2–3.5 Mb/s; joint generation split one player per GPU needs 149 Mb/s–389 Gb/s (Table 7). Measured between two machines, Solaris's exchange alone exceeds the 62.5-ms frame at every player count; WorldCast's takes under 1 ms per 1-s block (Table R1).

**W2a — FPS excludes waits; latency not evaluated.**
- Deployment reads the latest messages without waiting (App. B); lockstep is evaluation-only. With its waits counted, our optimized serving path (tiny decoder) runs 31.0–39.0 FPS per client for 2–16 clients on H200, vs. 18.7–23.4 in Table 7 (Table R2).
- Key press to screen: 0.73 s at the median (p95 1.18 s), set by the 1-s block as in any block-wise generator; the first frame follows the control read in under 0.2 s (page figure).

**W2b — Network delays not evaluated.**
Closed loop, paired vs. on-time messages: Rendered changes by −0.03 with random 0–2-block delays, 0.00 / −0.01 with 10% / 30% of messages lost, 0.00 with one client always a block late, −0.07 with every message a block late; image quality is unchanged.

**W2c — Hour-long runs: one client, recorded states for others, resets.**
New runs lift all three:
- All ten players as closed-loop clients, one hour, nothing reset: VBench imaging 71.1.
- Five closed-loop clients, three hours, nothing reset: imaging 71.5 / 72.7 / 73.1 in hours 1–3 (stills).
- Half a Dust2 match (13 min, 12 rounds), five clients, scene state never cleared: Unmatched 0.045, at its first-minute level; Rendered 0.836 (video).

**W2d — Firing missing in other views; why not evaluated?**
- Firing is modeled: the field carries each visible player's attack and reload keys and weapon (App. F). Page videos (attack held / attack zeroed / recording): the flash follows the attack channel.
- Weapons and aim are drawn; the flash is less frequent, as a close, visible firing player appears in only 0.5% of training frames.
- No detector reads firing or weapon reliably, even on recordings; readable attributes match near recording level: facing 0.825 vs. 0.868, airborne 0.85 vs. 0.89.

**Q2 — Four-step examples, evidence against the teacher, longest closed loop.**
- All supplementary videos except Section 01 (Table 1's bidirectional comparison) are four-step.
- Same windows and inputs, four-step minus teacher (Table R3): Rendered +0.033, Unmatched 0.000, VBench imaging +1.7, PSNR −0.14 dB, LPIPS +0.005; side-by-side videos on three maps.
- Longest closed loop: Section 02c (five clients, 54 s) and the three-hour run (W2c).

**Q3 — Consistency with more players than in training.**
- Each training sample is one player's window, and the field's size does not grow with the player count.
- A twenty-player world, twice any training match: Rendered within 0.013 of the ten-player world (paired), Unmatched unchanged (Table R4, stills).
- All ten players as closed-loop clients: Rendered 0.814 vs. 0.821 with up to seven.

**Minor — Fig. 2 is too packed.** Agreed; it is simplified in the revision.

**Revisions:** abstract, introduction and conclusion state the evaluation protocols and one GPU per player as a deployment choice; Table 7 adds rates with waits; Related Work expands the MultiGen comparison.

Anonymous page (figures, tables, videos): [index.html](index.html)

## Figures, tables and videos

- [How does the shared state update differ from MultiGen's?](index.html#r6): re-localization vs. a MultiGen-style update over a minute
- [Is joint generation's traffic a real cost across machines?](index.html#xfer): measured between two machines; computed for home broadband
- [What latency does a player see?](index.html#r1): one key press, from press to screen
- [How fast is it under each setting?](index.html#fps): Table 7 vs. the optimized serving path
- [Thirteen minutes without reset](index.html#long13): twelve rounds, five clients, nothing cleared
- [Is firing modeled?](index.html#r8): each example: attack held, attack zeroed, recording
- [How does the four-step model compare with its teacher?](index.html#r5): same windows, same inputs
- [Does it hold with more players?](index.html#r7): by concurrent clients and players in view
