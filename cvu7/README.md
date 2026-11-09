# Response to Reviewer cvu7

Unless marked as using the state model, consistency results use the recorded player positions, as in the paper's main tables. This controlled setup separates rendering from state estimation; under dense real-match controls (flicks, strafing, jumps), action-only rollouts depart from their recordings (App. G). Rates and latency below are measured on the full system.

**W1 & Q1 — What does WorldCast add over shared-state methods (MultiGen, MASS) in state update, visual conditioning and execution?**
WorldCast takes no global representation of the world as input. MultiGen is driven by a given map, ray-cast into each view, and advances poses by learned increments; MASS simulates a typed state of every entity on a server and renders it without RGB history. A WorldCast client receives only a camera-aligned field of the other players; geometry, appearance and physics come from the generator's learned priors.
- *State update.* Each client re-localizes its player from its own generated blocks (Eq. 4) every block, so error stays bounded. A MultiGen-style incremental update built in our stack drifts on the same generated videos: at 50–60 s it errs four times more (11.4 vs. 2.6 body widths); at 60 s, 5% vs. 39% of clients stay within two body widths (page: error curves, tracks).
- *Visual conditioning.* No map is read in deployment. Clients share their generated blocks (scene state), which keep what no map holds (smoke, open doors): switching them off costs 1.23 dB PSNR after 30 s (Table 18) and lowers agreement between closed-loop clients (Tables 4a, 19; page videos).
- *Player representation.* On the same backbone, data and budget, the camera-aligned field renders other players at 0.763, vs. 0.549 for late injection and 0.093 for per-player tokens (Table 5).
- *Execution.* No server and no engine: one generator per player publishes its own record and reads the others' at 2.2–3.5 Mb/s (Table 7), so a new player adds one generator and one message stream. Tables 1 and 10 compare this with joint generation on the same backbone, data and budget.

**W2 & Q2 — Cross-view agreement beyond placement (identity, appearance, actions); generated vs. extrapolated motion within a block.**
- Position is the core state; Table 15 scores its cross-view agreement (rays a median 17–27 u apart, under one body width).
- In CS2, identity is the team (T or CT), a channel of the field (App. F). Each visible player's team, facing, weapon and controls reach every client through the field, so no client infers who is who from pixels.
- On Table 2's videos, attributes are drawn at near recording level: same team in two views 0.987; facing 0.825 vs. 0.868; with the state model over whole rounds, a jumping player drawn airborne 0.85 vs. 0.89.
- Clips per state (jump, crouch, turn, move, weapon switch, firing) show the clients drawing the same action at the same moments (page: W2 & Q2).
- *Within a block:* standard dead reckoning (Bernier, 2001). Every client, j's own included, extrapolates player j from j's last estimate and the controls j publishes, so all generators receive one identical position for j. Where j's generated motion differs (e.g., a stop at a wall), the state model reads it from j's frames (Table 6, Blocked) and all clients correct to it at the next block: the discrepancy lasts at most one block and never accumulates.

**W2 & Q3 — End-to-end latency with lockstep waits; effect of delayed updates on quality and responsiveness.**
- Deployment does not wait (App. B): each client reads the others' latest messages, at 18.7–23.4 FPS for 2–16 clients (Table 7, H200), above 16-fps playback. Lockstep is used only in evaluation.
- With lockstep waits, our optimized serving path (tiny decoder) runs 31.0–39.0 FPS per client for 2–16 clients on H200, each bit-identical to the offline client (page: speed table).
- Input to display, waits included: 0.73 s median, 1.18 s p95. It is set by the 1-s block, as in any block-wise generator: a press waits for the next control read (0.50 s median), and the first frame follows in under 0.2 s (page: latency figure).
- *Delayed updates.* Own controls are read locally, so a late message never slows a client's response to its own input; it only shows that peer one block late. In closed loop, every message a block late lowers Rendered by 0.07; random 0–2-block delays by 0.03; losing 10% / 30% of messages by 0.00 / 0.01; one client always late by 0.00; image quality is unchanged (page: delay table).

**Revisions.** Sec. 3.1 will define real time and separate deployment from evaluation lockstep; Table 7, Related Work and App. G will add the results above.

Anonymous page (figures, tables, videos): [index.html](index.html)

## Figures, tables and videos

- [W1 & Q1 — What does WorldCast add over shared-state methods (MultiGen, MASS) in state update, visual conditioning and execution?](index.html#w1q1): position error over a minute, tracks, scene state
- [W2 & Q2 — Cross-view agreement beyond placement (identity, appearance, actions); generated vs. extrapolated motion within a block](index.html#w2q2): attributes; clips per action, including firing
- [W2 & Q3 — End-to-end latency with lockstep waits; effect of delayed updates on quality and responsiveness](index.html#w2q3): speed, latency, delayed messages
