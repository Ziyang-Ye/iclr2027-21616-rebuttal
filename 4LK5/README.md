# Response to Reviewer 4LK5

We thank the reviewer. Unless called closed-loop, consistency results give clients the recorded positions, as in the paper's main tables: this isolates the generator and shared state from the replaceable state model and keeps every view comparable with its recording, whereas dense real-match inputs (flicks, strafing, jumps) make action-only rollouts depart from their recordings (App. G). Rates and latency are of the full closed-loop system. All new numbers, tables and videos are on the anonymous page, one section per question below.

**W1 / Q1. Advance over MultiGen.** MultiGen is driven by a given map: each view conditions on a ray-cast of its geometry, and poses advance by learned increments. WorldCast has no global representation of the world: a client sees other players only through a camera-aligned field, and geometry, appearance and physics come from priors the generator learns. This removes the two limits MultiGen's Sec. 6 states:
- *Pose errors that "may accumulate over long rollouts".* Each client re-localizes its player from its own generated video (Eq. 4), and all clients re-anchor to it every block. A MultiGen-style update, trained like our state model, drifts: at 50–60 s it errs four times more (11.4 vs. 2.6 body widths); at 60 s, 5% vs. 39% of clients stay within two body widths.
- *Scene properties "not explicitly preserved".* Clients share their generated blocks instead of a map; switching this off costs 1.23 dB PSNR after 30 s (Table 18) and lowers agreement between closed-loop clients (App. E).

**W1. One GPU per player** is a deployment choice that networked play favors: a WorldCast client receives 2.2–3.5 Mb/s, while joint generation split one per GPU would need 149 Mb/s to 389 Gb/s (Table 7); measured across two machines on our cluster, Solaris's exchange alone exceeds the frame budget. The introduction will say so.

**W2. Qualified claims.**
- *Waits, latency.* Deployment does not wait (App. B): 18.7–23.4 FPS for 2–16 clients, all above 16-fps playback (Table 7; H200); every setting is on the page. Lockstep is evaluation-only; with its waits, our optimized serving path (tiny decoder) runs 16 clients on two H200 machines at 31.1 FPS, with input to display 0.73 s at the median, set by the 1-s block as in any block-wise generator.
- *Network delays.* In closed loop, Rendered drops by 0.07 with every message a block late, 0.03 with random 0–2-block delays, 0.00 / 0.01 with 10% / 30% of messages lost, 0.00 with one client always late; image quality is unchanged.
- *One hour.* Table 4b tests image stability, Table 4a closed-loop agreement over whole rounds. New: half a Dust2 match (13 min, 12 consecutive rounds), five clients, scene state never cleared: players stay correctly drawn throughout, with no drift over the 13 minutes (Rendered 0.836, Unmatched 0.045).

**W2. Shooting.** Firing is modeled: the field carries each visible player's attack and reload keys and weapon (App. F). Others' weapons and aim are drawn; the muzzle flash is drawn less reliably, as a close, visible firing player appears in only 0.5% of training frames. We do not score firing or weapon because no reader calibrates for them, even on recordings; where one does (crouching, facing, jumping), drawn attributes match the state near recording level (facing 0.825 vs. 0.868; a jumping player drawn airborne 0.85 vs. 0.89). Examples are on the page.

**Q2. Four-step model, longest example.** All supplementary videos except Section 01 (Table 1's comparison at the bidirectional stage the joint baselines reach) are four-step. On identical inputs the four-step model matches its bidirectional teacher: Rendered +0.033, Unmatched unchanged, imaging quality +1.7, frame metrics within 2%. Longest closed loop: Section 02c (five clients, 54 s), Fig. 7 (58.5 s), and now five closed-loop clients for three hours with nothing reset: image quality holds (VBench imaging 71.5 / 72.7 / 73.1 in hours 1–3; stills on the page).

**Q3. More players than in training.** Client count is not a training variable: each sample is one player's window of a ten-player match, and a client sees other players only through its camera-aligned field, whose size does not grow with the number of players. Doubling the world to twenty players leaves consistency unchanged: Rendered −0.013 (paired), Unmatched unchanged. With all ten as closed-loop clients, Rendered is 0.814 vs. 0.821 with up to seven, and an hour unreset keeps image quality (VBench 71.1).

**Minor. Fig. 2** will be simplified.

**Revisions:** the one-hour protocol and lockstep in abstract, introduction and conclusion; rates with waits in Table 7; MultiGen in Related Work.

Anonymous page (figures, tables, videos): [index.html](index.html)

## Figures, tables and videos

- [How does the shared state update differ from MultiGen's?](index.html#r6): re-localization vs. a MultiGen-style update over a minute
- [Is joint generation's traffic a real cost across machines?](index.html#xfer): measured between two machines; computed for home broadband
- [What latency does a player see?](index.html#r1): one key press, from press to screen
- [How fast is it under each setting?](index.html#fps): every measured setting: GPU, clients, decoder, code path
- [Thirteen minutes without reset](index.html#long13): twelve rounds, five clients, nothing cleared
- [Is firing modeled?](index.html#r8): four rendered examples, each beside its recording
- [How does the four-step model compare with its teacher?](index.html#r5): same windows, same inputs
- [Does it hold with more players?](index.html#r7): by concurrent clients and players in view
- [All numbers](index.html#nums): every number behind the reply, with intervals and sources
