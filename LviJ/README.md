# Response to Reviewer LviJ

We thank the reviewer. Unless called closed-loop, consistency results give clients the recorded positions, as in the paper's main tables: this isolates the generator and shared state from the replaceable state model and keeps every view comparable with its recording, whereas dense real-match inputs (flicks, strafing, jumps) make action-only rollouts depart from their recordings (App. G). Rates and latency are of the full closed-loop system. All new numbers, tables and videos are on the anonymous page, one section per question below.

**W1 / Q2. Sustained consistency with predicted states.** Rows given recorded positions (Tables 1–3, 15, 19) measure the architecture; predicted-state rows add the state model's error (Table 6), a component that can be replaced without retraining the generator (Sec. 3.5). The gap between the two rows is the state model's, not the shared-state architecture's. Table 4a already runs whole rounds of five closed-loop clients, given only their first frames and positions, with nothing reset.
- *Over elapsed time (new):* consistency does not decay: Rendered stays near 0.75 in every 10 s up to 100 s.
- *After 30 s.* Agreement is scored on pairs co-visible in the recording (App. E); closed-loop rollouts depart from their recordings (App. G), so many such pairs later view different places. Scene state still improves all four agreement measures, and in Table 19 it raises depth agreement after 30 s from 0.120 to 0.206.
- *Identity, facing, weapon, action.* In CS2, identity is the team (T or CT), a channel of the field (App. F); the generator never has to infer who is who from pixels, since each visible player's team, facing, weapon and controls reach every client through the field, and two views draw the same team for 0.987 of co-drawn players. New: facing, crouching and jumping are drawn as given almost as often as the recordings show them (facing 0.825 vs. 0.868; a jumping player drawn airborne 0.85 vs. 0.89). Supplementary Section 00 (Fig. 1, closed loop) already shows two clients drawing a third's jump; the page adds clips per state (jump, crouch, turn, move, weapon switch, firing).

**W1 / Q3. Without resets.** Table 4b's one-hour rollouts test image stability; Table 4a tests consistency.
- *New:* half a Dust2 match, twelve consecutive rounds (13 min), five clients, scene state never cleared: players stay correctly drawn throughout, with no drift over the 13 minutes (Rendered 0.836, Unmatched 0.045; video on the page).
- *New:* five closed-loop clients run three hours with nothing reset; image quality holds (VBench imaging quality 71.5 / 72.7 / 73.1 in hours 1–3; stills on the page).

**W2 / Q1. Deployment: rates, latency, controls within a block.** Deployment does not wait (App. B): each client reads the others' latest messages and runs at 18.7–23.4 FPS for 2–16 clients (Table 7; H200), above 16-fps playback; every measured setting is tabulated on the page. Lockstep is used only in evaluation, so that all clients read the same messages; with its waits, our optimized serving path (tiny decoder) runs 16 clients on two H200 machines at 31.1 FPS, and input to display is 0.73 s at the median and 1.18 s at p95. It is set by the block size, as in any block-wise generator: controls are read once per 1-s block, just before it is generated (App. B), so a press waits for the next block (0.50 s at the median); generating its first frame takes under 0.2 s.

**W2. Network delay and stale states.** Without lockstep a message can arrive a block late. A client's own controls are read locally, so a delay never slows its response to its own input; it only shows a peer one block late. In closed loop, Rendered drops by 0.07 with every message a block late, 0.03 with random 0–2-block delays per message, 0.00 / 0.01 with 10% / 30% of messages lost, and 0.00 with one client always a block late; PSNR moves by at most 0.05 dB.

**W2. Resources and traffic.** WorldCast stays above 16 FPS on one GPU per player at 2.2–3.5 Mb/s. Split one per GPU, joint generation must exchange 149 Mb/s to 389 Gb/s per GPU (Table 7). We now measured that exchange between two machines on our cluster: per denoising step it takes Solaris 69–551 ms for 2–16 players, above the 62.5-ms frame budget, and Gamma-World 7–17 ms; WorldCast's per-block exchange takes under 1 ms. On one GPU, joint generation falls to 2.2 FPS per player (Gamma-World) or runs out of memory (Solaris; Table 7).

**Revisions:** abstract, introduction and conclusion state the one-hour protocol and lockstep; Table 7 adds rates with waits, frames per GPU and measured transfer times.

Anonymous page (figures, tables, videos): [index.html](index.html)

## Figures, tables and videos

- [Do views agree on the same player?](index.html#xv): team, facing, pose: generated vs. recordings
- [Is firing modeled?](index.html#r8): each example: attack held, attack zeroed, recording
- [Thirteen minutes without reset](index.html#long13): twelve rounds, five clients, nothing cleared
- [What latency does a player see?](index.html#r1): one key press, from press to screen
- [How fast is it under each setting?](index.html#fps): Table 7 vs. the optimized serving path
- [Is joint generation's traffic a real cost across machines?](index.html#xfer): measured between two machines; computed for home broadband
- [All numbers](index.html#nums): every number behind the reply, with intervals and sources
