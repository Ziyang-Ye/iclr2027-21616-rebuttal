# Response to Reviewer LviJ

Most comparisons use recorded positions by design: as Reviewer cvu7 notes, evaluating recorded and predicted positions separately helps "distinguish rendering ability from state-estimation accuracy." Unless marked as using the state model, consistency results use recorded positions. Fully closed-loop results: Table 4a (five clients, whole rounds, no position resets), Table R4 (per 10 s to 100 s), Table R1 and Table R7.

**W1 & Q2 — Consistency with predicted states in the same scene, after 30 s, and beyond position.**
- *Why RotErr 55.6° and depth 0.120 are low (Table R1, Table R2).* These measures score pairs co-visible in the recording. Closed-loop clients leave the recorded trajectories (App. G), so after 30 s 82.7% of those pairs look at different places. For clients standing together (within 256 u, views overlap ≥ 0.5 both ways, Table R1), RotErr is 25.3° after 30 s (recording 16.3°, unrelated 59.8°) and depth 0.247 (0.397, 0.124). Depth is pose-sensitive: the recording's own depth at the closed-loop cameras scores 0.085 (closed loop 0.101). Scene state raises depth agreement from 0.092 to 0.120 (Table 19) and Mat. Pix. by 9.6–22.1K in every overlap group.
- *Position (Table R3).* On video generated with recorded positions, re-localizing every block holds median error to 2.6 body widths at 50–60 s, against 11.4 for a MultiGen-style update.
- *By elapsed time (Table R4).* Five closed-loop clients, whole rounds, nothing reset: Rendered is 0.75 in the first 10 s and 0.64–0.80 in every later 10 s up to 100 s; Unmatched 0.05, then 0.13–0.27.
- *Same player, compatible action.* In CS2, identity is the team (T/CT), a field channel (App. F). Readers calibrated on held-out recordings (balanced accuracy 0.79–0.99) score each player in one and two views. New (recordings in brackets): same team in two views 0.987 (0.994); facing 0.825 (0.868); crouch 0.81 (0.82, balanced). With the state model over whole rounds (Table R5), two views agree on team 0.987 (0.989) and on facing within 45° 0.875 (0.923); airborne drawn elevated 0.85 (0.89). Weapon and firing are shown, not scored.

**W1 & Q3 — Multi-client runs without position resets or memory clearing.**
- Yes: Table R4 (placement), Table R1 (cross-view).
- *15 min (Table R7):* five clients, one track, nothing reset, given-state rule (lower than the paper's: 0.690 vs. 0.836 on the 13-min run): Rendered 0.400 (state model) vs. 0.504 (recorded positions), no downward trend; Unmatched 0.231 vs. 0.091.
- *One hour, five clients, first 30 min (Table R7):* Rendered 0.572, 0.436, 0.357 per 10 min. Beyond 30 min, and with ten clients, agreement falls; the revision will say so.
- *One hour:* VBench imaging 71.5 (five clients), 71.1 (ten) (Table R8).
- Recorded positions, 13 min, scene state kept: Rendered 0.836, Unmatched 0.045 (Table R7b).

**W2 & Q1 — Synchronized FPS, action-to-display latency, controls within a block.**
- *FPS with lockstep waits (Table R9):* with engineering optimizations, H200, 2 / 4 / 8 / 10 / 16 clients: 28.2 / 27.5 / 24.5 / 24.1 / 24.2 with Table 7's full decoder, 38.9 / 37.6 / 32.8 / 32.2 / 30.7 with a tiny display decoder (latents unchanged). Table 7's 18.7–23.4 excludes waits; deployment does not wait (App. B).
- *Latency (new, page figure):* key press to screen at 2–16 clients, 0.73 s median, 1.18 s p95 with the tiny decoder; 0.94–0.95 / 1.40–1.41 s with Table 7's.
- *Controls within a block:* read once per 1-s block, just before it is generated; a press waits for the next read (median 0.50 s), and the block's first frame follows in under 0.2 s. Without lockstep a client reads the latest messages, so delay cannot slow its own input.

**W2 — Network delay, jitter, loss, stale states (new, Table R12, closed loop).** Change in Rendered vs. on-time messages: every message a block late −0.07; random 0–2-block delays −0.03; 10% / 30% lost 0.00 / −0.01; one client always a block late 0.00. PSNR changes by at most 0.075 dB, LPIPS by at most 0.003.

**W2 — Resource-normalized advantage; analytical baseline traffic.**
- *Measured (new, Table R13):* split across two machines, the exchange joint generation needs per denoising step takes 69–551 ms for Solaris (2–16 players), above the 62.5-ms frame budget, and 7–17 ms for Gamma-World; WorldCast's per-block exchange takes under 0.5 ms.
- Kept on one GPU, the released joint models fall to 2.2 FPS per player (Gamma-World) or run out of memory (Solaris) at 16 players (Table 7); WorldCast stays above 16 FPS per player at 2.2–3.5 Mb/s.

**Revisions:** abstract, introduction and conclusion will state the protocols and lockstep; Table 7 adds FPS with waits and measured transfers.

Anonymous page (figures, tables, videos): [index.html](index.html)

## Figures, tables and videos

- [W1 & Q2 — Consistency with predicted states in the same scene, after 30 s, and beyond position](index.html#w1q2): same scene; per 10 s; same player; firing
- [W1 & Q3 — Multi-client runs without position resets or memory clearing](index.html#w1q3): 13 minutes, minute by minute; image quality over one hour; videos
- [W2 & Q1 — Synchronized FPS, action-to-display latency, controls within a block](index.html#w2q1): speed and latency
- [W2 — Network delay, jitter, loss, stale states](index.html#w2-delay): delayed and lost messages
- [W2 — Resource-normalized advantage; analytical baseline traffic](index.html#w2-traffic): exchange time between two machines
