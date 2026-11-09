# Response to Reviewer cvu7

Unless marked as using the state model, consistency results use recorded positions, as in the paper's main tables, which separates rendering from state estimation. Rates and latency are measured on the full system.

**W1 & Q1 — What does WorldCast add over shared-state methods (MultiGen, MASS) in state update, visual conditioning and execution?**
The main advance: the shared state is observed from the generated video, not simulated, so what a player sees and what others draw of that player stay coupled. Clients also share their generated blocks, and no global representation of the world is an input. MultiGen is driven by a given map and advances poses by learned increments; MASS simulates a typed state of every entity on a server and renders it without RGB history. A WorldCast client receives only a camera-aligned field of the other players.
- *State update.* Each client re-localizes its player from its own generated blocks (Eq. 4) every block. MultiGen- and MASS-style updates built in our stack drift on the same generated videos: 11.4 and 12.6 vs. 2.6 body widths at 50–60 s; in closed loop the state's height stays within 0.26 body widths of the player's own frames, vs. 0.65 and 0.55 (Table R1b).
- *Visual conditioning.* No map is read in deployment. Clients share their generated blocks (scene state), which keep what no map holds (smoke, open doors): switching them off costs 1.23 dB PSNR after 30 s (Table 18) and lowers agreement between closed-loop clients (Tables 4a, 19; page videos).
- *Player representation.* On the same backbone, data and budget, the camera-aligned field renders other players at 0.763, vs. 0.549 for late injection and 0.093 for per-player tokens (Table 5).
- *Execution.* This follows from having no central process: MultiGen updates shared memory after all players generate each frame and MASS advances one state on a server, whereas each WorldCast generator publishes its own record and reads the others' at 2.2–3.5 Mb/s (Table 7).

**W2 & Q2 — Cross-view agreement beyond placement (identity, appearance, actions); generated vs. extrapolated motion within a block.**
- Position is the core state; Table 15 scores its cross-view agreement (rays a median 17–27 u apart, under one body width).
- In CS2, identity and appearance reduce to the team (T or CT), a channel of the field (App. F). Each visible player's team, facing, weapon and controls reach every client through the field.
- On Table 2's videos, attributes are drawn at near recording level: same team in two views 0.987; crouch 0.81 vs. 0.82; whole rounds, state model: two-view facing 0.875 vs. 0.923, airborne 0.85 vs. 0.89.
- Clips per state show clients drawing the same action at the same moments (page).
- *Within a block:* standard dead reckoning (Bernier, 2001). Every client, j's own included, extrapolates player j from j's last estimate and the controls j publishes, so clients reading that record get one position for j. Where j's generated motion differs (e.g., a stop at a wall), the state model reads it from j's frames (Table 6, Blocked) and all clients correct to it at the next block. The position others use differs from j's own estimate by a median 0.35 body widths at a block's first latent and 0.90 at its last (Table R3b), and never accumulates.

**W2 & Q3 — End-to-end latency with lockstep waits; effect of delayed updates on quality and responsiveness.**
- Deployment does not wait (App. B): each client reads the others' latest messages, hence Table 7 excludes waits. Lockstep is used only in evaluation.
- With lockstep waits and Table 7's full decoder, engineering optimizations bring the serving path to 24.1–28.2 FPS for 2–16 clients on H200 (30.7–39.0 with a tiny display decoder), each bit-identical to the offline client (page).
- A 16-frame block takes 570–660 ms with waits. Input to display, waits included: 0.73 s median, 1.18 s p95 with the tiny decoder, 0.94 / 1.40 s with Table 7's (page figure): a press waits for the next control read (0.50 s median), and the first frame follows in under 0.2 s. A's generated motion reaches B's screen in 1.74 s (p95 2.20 s, 8 clients).
- *Delayed updates.* Own controls are read locally, so a late message never slows a client's response to its own input; it only shows that peer one block late. In closed loop, every message a block late lowers Rendered by 0.07; random 0–2-block delays by 0.03; losing 10% / 30% of messages by 0.00 / 0.01; one client always late by 0.00; image quality is unchanged (page).

**Revisions.** Sec. 3.1 will separate deployment from evaluation lockstep; Table 7, Related Work and App. G will add these results.

Anonymous page (figures, tables, videos): [index.html](index.html)

## Figures, tables and videos

- [W1 & Q1 — What does WorldCast add over shared-state methods (MultiGen, MASS) in state update, visual conditioning and execution?](index.html#w1q1): position error over a minute, tracks, scene state
- [W2 & Q2 — Cross-view agreement beyond placement (identity, appearance, actions); generated vs. extrapolated motion within a block](index.html#w2q2): attributes; clips per action, including firing
- [W2 & Q3 — End-to-end latency with lockstep waits; effect of delayed updates on quality and responsiveness](index.html#w2q3): speed, latency, delayed messages
