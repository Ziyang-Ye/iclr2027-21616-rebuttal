# Response to Reviewer cvu7

As in the paper's main tables, consistency results use recorded positions unless marked as using the state model. Rates and latency are measured on the full system.

**W1 & Q1 — What does WorldCast add over shared-state methods (MultiGen, MASS) in state update, visual conditioning and execution?**
The main advance is a closed loop between video and state: every block, each client re-localizes its player from the frames it generated, so what happens in the generated world (a stop at a wall, a change of height) enters the shared state, and what others draw of a player stays coupled to what that player sees. MASS advances a hand-specified typed state with a logic engine on a server and never reads the rendered frames, which suits games whose rules a schema covers; MultiGen integrates pose increments on a given map.
- *State update.* Built in our stack, MultiGen- and MASS-style updates drift on videos generated with recorded positions: 11.4 and 12.6 vs. 2.6 body widths at 50–60 s; in closed loop the state's height is a median 0.26 body widths from the player's frames, vs. 0.65 and 0.55 (Table R1b).
- *Shared appearance.* Over a long rollout each causal generator drifts in its own way, and a shared map and poses cannot bring the views back to one world: MultiGen notes that properties outside the map are "not explicitly preserved" and reports no long-rollout measurement. WorldCast clients share the blocks they generate (scene state), so the drift itself is shared and the generated frames are part of the loop. With it off, PSNR falls 1.23 dB after 30 s (Table 18) and agreement between closed-loop clients drops (Table 4a); where a client reads another's entry, matched pixels rise by 23.2K (Table R1c).
- *Execution.* MultiGen's players read and write one shared memory after all have generated each frame; MASS steps one state on a server. Each WorldCast client publishes only its own record and, in deployment, reads the others' latest without waiting.

**W2 & Q2 — Cross-view agreement beyond placement (identity, appearance, actions); generated vs. extrapolated motion within a block.**
- Position is the core state; Table 15 scores its cross-view agreement (rays a median 17–27 u apart, under one body width).
- In CS2, identity and appearance reduce to the team (T or CT), a channel of the field (App. F). Each visible player's team, facing, weapon and controls reach every client through the field.
- On Table 2's videos, attributes are drawn at near recording level: same team in two views 0.987; crouch 0.81 vs. 0.82; whole rounds, state model: two-view facing 0.875 vs. 0.923, airborne 0.85 vs. 0.89.
- Clips per state show clients drawing the same action at the same moments (page).
- *Within a block:* standard dead reckoning (Bernier, 2001). Every client, j's own included, extrapolates player j from j's last estimate and the controls j publishes, so clients reading that record get one position for j. Where j's generated motion differs (e.g., a stop at a wall), the state model reads it from j's frames (Table 6, Blocked) and all clients correct to it at the next block. The position others use differs from j's own estimate by a median 0.35 body widths at a block's first latent and 0.90 at its last (Table R3b), and resets next block.

**W2 & Q3 — End-to-end latency with lockstep waits; effect of delayed updates on quality and responsiveness.**
- Deployment does not wait (App. B): each client reads the others' latest messages, hence Table 7 excludes waits. Lockstep is used only in evaluation.
- With lockstep waits and Table 7's full decoder, engineering optimizations bring the serving path to 24.1–28.2 FPS for 2–16 clients on H200 (30.7–38.9 with a tiny display decoder), each bit-identical to the offline client (Table R5).
- A 16-frame block takes 570–660 ms with waits. Input to display, waits included: 0.73 s median, 1.18 s p95 with the tiny decoder, 0.94 / 1.40 s with Table 7's (Table R6): a press waits for the next control read (0.50 s median), and the first frame follows in under 0.2 s. A's generated motion reaches B's screen in 1.74 s (p95 2.20 s, 8 clients).
- *Delayed updates.* Own controls are read locally, so a late message never slows a client's response to its own input; it only shows that peer one block late. In closed loop, every message a block late lowers Rendered by 0.07; random 0–2-block delays by 0.03; losing 10% / 30% of messages by 0.00 / 0.01; one client always late by 0.00; image quality is unchanged (Table R7).

**Revisions.** Sec. 3.1 will separate deployment from evaluation lockstep; Table 7, Related Work and App. G will add these results.

Anonymous page (figures, tables, videos): [index.html](index.html)

## Figures, tables and videos

- [W1 & Q1 — What does WorldCast add over shared-state methods (MultiGen, MASS) in state update, visual conditioning and execution?](index.html#w1q1): position error over a minute, tracks, scene state
- [W2 & Q2 — Cross-view agreement beyond placement (identity, appearance, actions); generated vs. extrapolated motion within a block](index.html#w2q2): attributes; clips per action, including firing
- [W2 & Q3 — End-to-end latency with lockstep waits; effect of delayed updates on quality and responsiveness](index.html#w2q3): speed, latency, delayed messages
