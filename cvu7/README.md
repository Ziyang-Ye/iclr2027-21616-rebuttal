# Response to Reviewer cvu7

We thank the reviewer. Unless called closed-loop, consistency results give clients the recorded positions, as in the paper's main tables: this isolates the generator and shared state from the replaceable state model and keeps every view comparable with its recording, whereas dense real-match inputs (flicks, strafing, jumps) make action-only rollouts depart from their recordings (App. G). Rates and latency are of the full closed-loop system. All new numbers, tables and videos are on the anonymous page, one section per question below.

**W1 / Q1. Main advance over MultiGen and MASS.** WorldCast's shared world follows what clients actually generate. Each client re-localizes its player from its own generated blocks (Eq. 4) and shares those blocks; no map geometry is read in deployment. MultiGen instead advances poses by learned increments over a given map; MASS advances a typed state on a server. MultiGen's own Sec. 6 names accumulating pose error and unpreserved scene properties as its limits; re-localization and shared blocks address exactly these. Three controlled results show each choice matters (same backbone, data, budget):
- *State update.* A MultiGen-style update built in our stack drifts: at 50–60 s it errs four times more than Eq. 4 (11.4 vs. 2.6 body widths), and at 60 s only 5% of its clients stay within two body widths, vs. 39%.
- *Conditioning.* A camera-aligned player field is what makes others drawable: Rendered 0.763 vs. 0.093 for per-player state tokens (Table 5).
- *Shared views.* Sharing generated blocks keeps appearance consistent: switching scene state off costs 1.23 dB PSNR after 30 s (Table 18) and lowers closed-loop agreement between clients (App. E).

Execution is decentralized: one generator per player, no central process; each client receives only 2.2–3.5 Mb/s (Table 7), so adding a player adds one generator and one message stream, not cross-GPU attention. Tables 1 and 10 compare decoupled clients with joint generation on the same backbone, data and budget.

**W2 / Q2. Identity, appearance, actions; within a block.** Position is the core state, and Table 15 scores its cross-view agreement: two clients draw a third client's player with rays a median 17–27 u apart, under one body width. In CS2, identity is the team (T or CT), a channel of the field (App. F). The generator never has to infer who is who from pixels: each visible player's team, facing, weapon and controls reach every client through the field. On Table 2's videos, the other attributes are drawn at near recording level:
- two views draw the same team for 0.987 of co-drawn players;
- facing, crouching and jumping match the given state almost as often as the recordings themselves (facing 0.825 vs. 0.868; a jumping player drawn airborne 0.85 vs. 0.89)

*Within a block.* This is standard dead reckoning (Bernier, 2001): every client, j's own included, extrapolates player j from j's last estimate and the controls j publishes before generating, so all generators receive one identical position for j. Where j's frames depart from the extrapolation, as at a wall, the gap is small (a median 0.35 body widths a quarter-second in), and at the next block all clients correct to j's re-localized estimate, so the correction stays bounded and never accumulates.

**W2 / Q3. Latency with waits; delayed updates.** Deployment does not wait (App. B): each client reads the others' latest messages and runs at 18.7–23.4 FPS for 2–16 clients (Table 7), above 16-fps playback. Lockstep is used only in evaluation, so that all clients read the same messages; even with its waits, every rate for up to ten clients stays above 16 FPS. On our serving path, input to display is 0.73 s at the median and 1.18 s at p95, waits included. It is set by the block size, as in any block-wise generator: controls enter once per 1-s block, so a press waits for the next block (0.50 s at the median); generating its first frame takes under 0.2 s.
- *Delayed updates.* Own controls are read locally, so a late message never slows a client's response to its own input; it only shows that peer one block late. Even with every message one block late, closed-loop Rendered drops by just 0.07, holding only others' controls by 0.035, and image quality is unchanged.

**Revisions.** Sec. 3.1 will define real time, separate the deployed protocol from evaluation lockstep, and state the within-block protocol; Table 7, Related Work and App. G will add the above.

We will add to this thread serving-path rates for 8–16 clients and runs with random delays, message loss, or one client always a block late.

Anonymous page (figures, tables, videos): [index.html](index.html)

## Figures, tables and videos

- [How does the shared state update differ from MultiGen's?](index.html#r6): re-localization vs. a MultiGen-style update over a minute
- [Do views agree on the same player?](index.html#xv): team, facing, pose: generated vs. recordings
- [What latency does a player see?](index.html#r1): one key press, from press to screen
- [All numbers](index.html#nums): every number behind the reply, with intervals and sources
