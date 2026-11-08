# Response to Reviewer LviJ

We thank the reviewer. Unless run with the state model, consistency results give clients the recorded positions, as in the paper's main tables. This controlled design, credited by Reviewer cvu7, separates rendering from state estimation and locates each source of error; with dense real-match inputs (flicks, strafing, jumps), action-only rollouts depart from their recordings (App. G). Rates and latency are of the full system. New results are on the anonymous page, one section per point.

**W1 & Q2 — Whole-round consistency with predicted states, after 30 s, and beyond position.**
- *Whole rounds, closed loop.* Table 4a runs five closed-loop clients through whole rounds from their first frames and positions, nothing reset. Re-localizing each player from its generated video every block keeps the state model's error bounded: 2.6 body widths at 50–60 s (Table 6), against 11.4 for a MultiGen-style update that integrates learned increments (new).
- *By elapsed time (new).* Closed-loop Rendered is 0.75 in the first 10 s and 0.64–0.80 in every later 10 s up to 100 s.
- *After 30 s.* Agreement is scored on pairs co-visible in the recording (App. E), which closed-loop rollouts leave. Scene state still improves all four measures (Table 4a: Mat. Pix. 31.1→44.3K, Inliers 18.8→27.7, RotErr 59.1°→55.6°, depth 0.092→0.120, Table 19).
- *Identity, facing, weapon, action.* In CS2, identity is the team (T/CT), a field channel (App. F) that, with facing, weapon and controls, reaches every client. New (recordings in brackets): same team in two views 0.987 (0.994); facing as given 0.825 (0.868); airborne drawn elevated 0.85 (0.89). In closed-loop whole rounds, two views agree on a player's facing within 45° for 0.875 of pairs (0.923). Clips per action (jump, crouch, turn, move, weapon switch, firing) are on the page.

**W1 & Q3 — Multi-client runs without position resets or memory clearing.**
- Table 4b tests image stability; Table 4a tests consistency without resets.
- *New:* half a Dust2 match, 12 consecutive rounds (13 min), five clients, scene state never cleared: Rendered 0.836, Unmatched 0.045, at its first-minute level throughout (video).
- *New:* five closed-loop clients, three hours, nothing reset: VBench imaging quality 71.5 / 72.7 / 73.1 in hours 1–3 (stills).

**W2 & Q1 — Synchronized FPS, action-to-display latency, controls within a block.**
- *FPS with lockstep waits (new, Table R2):* 39.0 / 37.2 / 32.9 / 31.0 / 31.1 per client for 2 / 4 / 8 / 10 / 16 clients on H200, on our optimized serving path (tiny decoder for display; latents unchanged). Table 7's 18.7–23.4 is the evaluation code with the full decoder, excluding waits; deployment does not wait (App. B).
- *Latency (new, page figure):* key press to screen 0.73 s median, 1.18 s p95, at 8 and 16 clients.
- *Controls within a block:* read once per 1-s block, just before it is generated (App. B); a press waits for the next read (median 0.50 s), and the block's first frame follows in under 0.2 s. A client's own controls are local, so network delay never slows its response to its own input.

**W2 — Network delay, jitter, loss, stale states (new, Table R1, closed loop).** Change in Rendered vs. on-time messages: every message a block late −0.07; random 0–2-block delays −0.03; 10% / 30% lost 0.00 / −0.01; one client always a block late 0.00. PSNR changes by at most 0.075 dB, LPIPS by at most 0.003.

**W2 — Resource-normalized advantage; analytical baseline traffic.**
- *Measured (new, Table R3):* split across two machines, the exchange joint generation needs per denoising step takes 69–551 ms for Solaris (2–16 players), above the 62.5-ms frame budget, and 7–17 ms for Gamma-World; WorldCast's per-block exchange takes under 0.5 ms.
- Kept on one GPU, the released joint models fall to 2.2 FPS per player (Gamma-World) or run out of memory (Solaris) at 16 players (Table 7); WorldCast stays above 16 FPS per player at 2.2–3.5 Mb/s.

**Revisions:** abstract, introduction and conclusion will state the evaluation protocols and lockstep; Table 7 will add FPS with waits and the measured transfer times.

Anonymous page (figures, tables, videos): [index.html](index.html)

## Figures, tables and videos

- [Do views agree on the same player?](index.html#xv): team, facing, pose: generated vs. recordings
- [Is firing modeled?](index.html#r8): each example: attack held, attack zeroed, recording
- [Thirteen minutes without reset](index.html#long13): twelve rounds, five clients, nothing cleared
- [What latency does a player see?](index.html#r1): one key press, from press to screen
- [How fast is it under each setting?](index.html#fps): Table 7 vs. the optimized serving path
- [Is joint generation's traffic a real cost across machines?](index.html#xfer): measured between two machines; computed for home broadband
