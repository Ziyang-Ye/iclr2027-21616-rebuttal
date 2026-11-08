# Response to Reviewer cvu7

We thank the reviewer. Unless called closed-loop, consistency results give clients the recorded positions, as in the paper's main tables: this isolates the generator and shared state from the replaceable state model and keeps every view comparable with its recording, whereas dense real-match inputs (flicks, strafing, jumps) make action-only rollouts depart from their recordings (App. G). Rates and latency are of the full closed-loop system. All new numbers, tables and videos are on the anonymous page, one section per question.

**W1 / Q1. Main advance over MultiGen and MASS.** WorldCast needs no global representation of the world. MultiGen is driven by a given map, ray-cast into each view, and advances poses by learned increments; MASS simulates a typed state of every entity on a server and renders it without RGB history. A WorldCast client receives only a camera-aligned field of the other players; geometry, appearance and physics come from priors the generator learns, and the shared world is what the clients generate.
- *State update.* Each client re-localizes its player from its own generated blocks (Eq. 4), and all clients re-anchor to it every block, so error does not accumulate. A MultiGen-style incremental update built in our stack drifts: at 50–60 s it errs four times more (11.4 vs. 2.6 body widths); at 60 s, 5% vs. 39% of clients stay within two body widths.
- *Visual conditioning.* No map is read in deployment. Clients share their generated blocks (scene state), which keep what no map holds (smoke, open doors): switching them off costs 1.23 dB PSNR after 30 s (Table 18) and lowers agreement between closed-loop clients (App. E).
- *Execution.* No server and no engine, unlike MASS: one generator per player publishes its own record and reads the others' at 2.2–3.5 Mb/s (Table 7), so adding a player adds one generator and one message stream. Tables 1 and 10 compare this with joint generation on the same backbone, data and budget.

**W2 / Q2. Identity, appearance, actions; within a block.** Position is the core state, and Table 15 scores its cross-view agreement (rays a median 17–27 u apart, under one body width). In CS2, identity is the team (T or CT), a channel of the field (App. F). The generator never has to infer who is who from pixels: each visible player's team, facing, weapon and controls reach every client through the field. On Table 2's videos, the other attributes are drawn at near recording level:
- two views draw the same team for 0.987 of co-drawn players;
- facing, crouching and jumping match the given state almost as often as the recordings themselves (facing 0.825 vs. 0.868; a jumping player drawn airborne 0.85 vs. 0.89).

Supplementary Section 00 (Fig. 1, closed loop) already shows two clients drawing a third's jump; the page adds clips per state (jump, crouch, turn, move, weapon switch, firing).

*Within a block.* This is standard dead reckoning (Bernier, 2001): every client, j's own included, extrapolates player j from j's last estimate and the controls j publishes before generating, so all generators receive one identical position for j. At the next block all clients correct to j's re-localized estimate, so the correction stays bounded and never accumulates.

**W2 / Q3. Latency with waits; delayed updates.** Deployment does not wait (App. B): each client reads the others' latest messages and runs at 18.7–23.4 FPS for 2–16 clients (Table 7; H200, full decoder), above 16-fps playback. Lockstep is used only in evaluation, so that all clients read the same messages; even with its waits, every rate for up to ten clients stays above 16 FPS. On our optimized serving path (tiny decoder), 16 clients on two H200 machines run at 31.1 FPS with lockstep waits, each bit-identical to the offline client; input to display is 0.73 s at the median and 1.18 s at p95, waits included. The page sets them against Table 7. It is set by the block size, as in any block-wise generator: controls enter once per 1-s block, so a press waits for the next block (0.50 s at the median); generating its first frame takes under 0.2 s.
- *Delayed updates.* Own controls are read locally, so a late message never slows a client's response to its own input; it only shows that peer one block late. In closed loop, every message a block late lowers Rendered by 0.07; random 0–2-block delays per message by 0.03; losing 10% or 30% of messages by 0.00 / 0.01; one client always late by 0.00. Image quality is unchanged.

**Revisions.** Sec. 3.1 will define real time, separate the deployed protocol from evaluation lockstep, and state the within-block protocol; Table 7, Related Work and App. G will add the above.

Anonymous page (figures, tables, videos): [index.html](index.html)

## Figures, tables and videos

- [How does the shared state update differ from MultiGen's?](index.html#r6): re-localization vs. a MultiGen-style update over a minute
- [Do views agree on the same player?](index.html#xv): team, facing, pose: generated vs. recordings
- [What latency does a player see?](index.html#r1): one key press, from press to screen
- [How fast is it under each setting?](index.html#fps): Table 7 vs. the optimized serving path
