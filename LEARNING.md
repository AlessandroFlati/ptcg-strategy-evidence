# Separate post-submission recurrent learning

## Exact comparison

The Lucario experiment uses the same 60 card counts as the submitted hybrid, but a different policy lineage. Its parent is a replay-imitation model from 150 games of an earlier Lucario target-router policy. Recurrent V-trace training uses terminal-only reward. This is not reinforcement learning (RL) applied to the submitted hybrid checkpoint, and the submitted hybrid is not an evaluation arm.

The published V-trace method addresses off-policy correction in an actor-learner architecture. Our local use does not inherit the original paper's scale or performance claims. [Espeholt et al., IMPALA](https://arxiv.org/abs/1802.01561)

## What the recurrent learner actually implements

### Architecture and comparator map

| Component or role | Submitted neural proposal | Later Lucario recurrent learner |
|---|---|---|
| State/option attention | 8 layers, width 512, 8 heads | 2 layers, width 128, 4 heads |
| Sequence memory | No recurrent state branch in the submitted checkpoint | Gated recurrent unit (GRU) over the latest sixteen selected own-action tokens |
| Training signal | Recorded replay choices | Replay initialization, then terminal-outcome actor-critic learning with regularization |
| Output used in play | Option ranking; eligible single-choice alternative enters terminal comparison | History-conditioned option scores and a sequential multi-selection decoder |
| Value estimate | No learned value head used by the submitted terminal arbiter | Value head supplies actor-critic training targets, not the submitted search's terminal payoff |
| Deployment status | Part of the ranked hybrid | Separate post-submission local policy; not integrated into that hybrid |
| Actual evaluation comparator | Hybrid compared with its rule/priority baseline | Recurrent policy compared with its separate replay-imitation parent |

GRU means gated recurrent unit; BC means behavioral cloning, or imitation of recorded actions. Both hybrid comparison arms already include replay-derived priorities. The later replay-BC parent is a different model, initialized from 150 games of an earlier Lucario policy. Ten opponent references, twelve hidden-state samples and two proposed moves are separate objects; none denotes a team of learned child agents. Network size alone cannot rank these two systems because their training, integration and evaluation designs differ.

### Recurrent mechanism

This is a different network from the submitted eight-layer, width-512, eight-head proposal model. Its replay-initialized state/option encoder has two attention layers, width 128 and four heads. A gated recurrent unit (GRU) encodes a bounded buffer of the latest sixteen selected own-action tokens. The buffer is rebuilt under the current weights during learning, rather than reusing a stale recurrent latent produced by an earlier actor.

The history input is not a full transcript: the rollout appends the controlled policy's selected options, not intervening opponent actions. It resets at each game. Board information can still reflect opponent actions, but this history mechanism is not best-of-three (BO3) opponent adaptation or evidence of perfect recall.

The actor adds history-conditioned residual scores to the trainable option encoder's scores. A value head estimates return from pooled legal-option context and history. For multi-selection decisions, a canonical sequential decoder enforces legal minimum/maximum counts and an admissible stop action. A frozen copy of the imitation model supplies a regularization reference; putting the trainable base into evaluation mode disables dropout but does not freeze its gradients.

V-trace compares the current policy's probability of each recorded action sequence with its rollout-policy probability, clips importance ratios, and computes corrected value targets and policy advantages within each game. The configured discount is 0.997; value and policy learning use terminal reward, with entropy and imitation-reference penalties. Terminal-only reward does not mean undiscounted optimization or an absence of regularization. The retained experiment used four rollout workers and an RTX 5090, not finalist H100 hardware.

The locked comparison supports this training package over its own parent. It does not isolate the contribution of recurrence, correction, model size, imitation initialization, or GPU quantity. Those require matched ablations. In particular, retaining own-action history must not be credited with using earlier BO3 game logs.

## What the correction and decoder mean

For a recorded decision, the learner recomputes the probability of the selected sequence under its current weights and compares it with the probability recorded by the rollout policy. Their ratio supplies separate capped weights for the value correction, backward trace and policy advantage; the retained Lucario protocol sets all three caps to one. The sequence probability includes the conditional choices and any explicit stop, not a product of independent per-card scores. The implementation also bounds the log-ratio before exponentiation for numerical stability.

With equal current and rollout probabilities and unit caps, the complete-game target reduces to the discounted return. As a synthetic arithmetic example, three decisions with rewards 0, 0, 1 and discount 0.5 give targets 0.25, 0.5, 1. This is an explanatory fixture, not an observed game or this experiment's discount setting. The actual configured discount is 0.997. Targets stop at each game boundary; records must remain chronological within each game.

The original V-trace analysis distinguishes value-ratio truncation from trace truncation; clipping is not an assertion of unbiased current-policy evaluation. We do not transfer its tabular convergence analysis into a guarantee for our neural, bounded-history agent. [Espeholt et al., section 4 and Appendix A](https://proceedings.mlr.press/v80/espeholt18a/espeholt18a.pdf)

Third-party replays can supply imitation examples without supplying the original policy's action probabilities. V-trace cannot reconstruct those missing probabilities merely from the selected moves, nor create evidence for unobserved alternatives. Reusing such logs for corrected RL would require a justified behavior-probability and action-support treatment. Our owned rollouts record their own behavior probabilities.

The canonical decoder visits increasing option positions, reserves enough remaining choices to satisfy the minimum, and stops at the maximum or an admissible stop. Its choice distribution is over permitted subsets represented in this order. Bounded synthetic checks enumerated all subsets of a four-option menu across fifteen minimum/maximum combinations and verified probability normalization and reconstruction. They used fixed logits and a zero stop-residual stub; they do not establish full-network or simulator correctness.

The history buffer stores selected option tokens: selecting several options can append several tokens, while an empty selection appends none. Thus sixteen tokens need not represent sixteen decisions or turns. The final evaluator requests deterministic stepwise decoding. Its win rates describe that execution policy, not repeated sampling from the training distribution; greedy stepwise decoding need not maximize the probability of the whole subset.

## Locked local final evaluation

Each of 5,000 pairs compares candidate and replay-BC parent against the same local opponent and randomized scenario. There are 500 pairs per opponent and 2,500 per simulator player position. Slots are not synonymous with turn order. The ten local opponent programs are the same modules the hybrid confirmation used, byte-identical to the archived copies. Both tests allocated equal pair counts within their panels: 50 per opponent for the hybrid and 500 for RL. The hybrid aggregates with historical deck-frequency weights; RL aggregates opponents equally. Seeds and comparison arms also differ. One shared damage helper (`engine/exact_damage.py`) had gained two damage rules by the time of this evaluation, and two rule-based opponents (Archaludon and Mega Starmie) import it, so their decisions are not shown to be identical across the two evaluations; the six mined proxies and the other two rule-based opponents do not import it. Do not compare aggregate win rates across the two evaluations.

| Measure | Recurrent candidate | Replay-BC parent |
|---|---:|---:|
| Wins | 2,284 | 1,614 |
| Draws | 6 | 3 |
| Losses | 2,710 | 3,383 |
| Total complete games | 5,000 | 5,000 |
| Win percentage | 45.68% | 32.28% |
| Mean reward: loss/draw/win = -1/0/+1 | -0.0852 | -0.3538 |
| Recorded timeouts / illegal actions | 0 / 0 | 0 / 0 |

The candidate is better in 1,135 pairs, worse in 462 and equal in 3,403. The paired mean reward difference is +0.2686, paired standard error (SE) 0.015493831668246328, with a 10,000-resample paired-bootstrap 95% interval of [+0.2384, +0.2992] using seed 72999. The candidate's absolute win percentage remains below 50%.

The mean difference can be recovered from each arm's (wins - losses) / 5,000. On win=1/draw=0.5/loss=0 scores, divide the reward difference and interval by two: +13.43 score percentage points, interval [+11.92, +14.96]. The actual win-rate difference is +13.40 points because the arms have different draw counts. Neither is +26.86 win-rate points. Paired SE and bootstrap require paired outcomes, not just marginal win totals.

| Local opponent | Pairs | Mean paired reward difference |
|---|---:|---:|
| Archaludon | 500 | +0.192 |
| Dudunsparce / Mega Lopunny | 500 | +0.508 |
| Spidops | 500 | +0.300 |
| Starmie | 500 | +0.166 |
| Alakazam | 500 | +0.332 |
| Crustle | 500 | +0.170 |
| Dragapult | 500 | +0.444 |
| Garchomp | 500 | +0.252 |
| Grimmsnarl | 500 | +0.154 |
| Lucario | 500 | +0.168 |

Player-position differences are +0.2628 and +0.2744. Positive means across these groups do not establish independence from all initial states or robustness to unseen opponents. The comparison is against a relatively weak replay-BC parent; the next useful test is against a qualified stronger comparator under matched conditions.

## Selection, amendments and identity

The retained selection record precedes the final evaluation and declares the final bank unopened. Declared training, development and final seed intervals are disjoint; actual final/probe records also have no seed overlap. Our revision rechecked final/checkpoint hashes, all paired deltas, aggregate SE, bootstrap endpoints, per-opponent means, balance and safety fields. It did not rerun the games.

The schedule was extended by 24 hours to restore learner time lost to supervision failures. Development improvements had already been observed when that extension was authorized. The records preserve the original protocol and explicit amendments; this is not an unchanged prospectively fixed 72-hour experiment. Held-out final seeds do not erase adaptive research history, comparator weakness or shared-roster limitations.

Candidate checkpoint SHA-256: `c813a3e080cfd47edd2584f24f30bf501d9cb91a37dcb72b75508a606b1436a2`.

Final evaluation SHA-256: `2179efe52217ed37e266aed1f211665f448c16745931d0c970f6d1ae54f27c43`.

Hashes identify retained artifacts; this companion does not distribute them or establish public train-from-raw reproduction.

## Reading paired results without conflating the experiments

There are three comparison units: two proposals inside one searched decision; two complete hybrid/baseline games in a confirmation pair; and two complete recurrent/parent games in a later RL pair. Their outcome totals are not interchangeable. The [published hybrid evaluation](https://github.com/AlessandroFlati/ptcg-strategy-evidence/blob/main/EVALUATION.md) already provides the historical-weight estimator and opponent aggregates. The following adds a paired-outcome reading aid and a separately labelled same-record diagnostic.

For the hybrid's 500 confirmation pairs, the complete-game outcomes were:

| Hybrid outcome | Baseline win | Baseline loss | Total |
|---|---:|---:|---:|
| Win | 295 | 87 | 382 |
| Loss | 35 | 83 | 118 |
| Total | 330 | 170 | 500 |

There were no drawn games. The 378 equal pairs are 295 double wins plus 83 double losses. The pooled difference is (87 - 35) / 500 = 10.4 percentage points. Since each opponent has fifty pairs, this is an equal-opponent mixture. Its fixed-opponent-stratum standard error is 2.153 points. The main report retains the historically weighted +12.587 points / SE 4.026 because that was the specified primary estimand; choosing the smaller standard error after seeing outcomes would change the question being answered. These summaries reuse the same records.

For the separate Lucario RL experiment, retain the draw cells:

| Recurrent outcome | Parent loss | Parent draw | Parent win | Total |
|---|---:|---:|---:|---:|
| Loss | 2,250 | 1 | 459 | 2,710 |
| Draw | 4 | 0 | 2 | 6 |
| Win | 1,129 | 2 | 1,153 | 2,284 |
| Total | 3,383 | 3 | 1,614 | 5,000 |

For example, a recurrent win paired with a parent loss contributes +2 on the -1/0/+1 reward scale, while a draw paired with a loss contributes +1. Summing all cells with their reward differences gives 1,343 / 5,000 = +0.2686. This explains why the 1,135 better and 462 worse pairs alone need their outcome types to reconstruct the reward difference. The same data give +13.43 points on a win-one/draw-half scale and +13.40 actual win-rate points.

The reported primary uncertainty remains the paired bootstrap in the final record. As a post-analysis check treating the ten equally allocated opponent groups as fixed strata, the mean remains +0.2686 and the stratified SE is 0.0154153, compared with pooled SE 0.0154938. A normal 95% interval using that stratified SE is [+0.23839, +0.29881]. Its numerical similarity to the original bootstrap interval is useful arithmetic context, not independent replication, a selection-adjusted interval or unseen-opponent validation.

## Missing coverage, safety and objective boundaries

For the hybrid only, write an imagined same-metric population difference as `p * covered_delta + (1 - p) * missing_delta`. Using retained coverage p = 0.77996255 and covered_delta = 0.12586735, break-even would require approximately -44.62 points on the missing portion. Allowing every missing outcome difference in [-1, +1] gives a broad point-estimate sensitivity range of -12.19 to +31.82 points. This is not a confidence interval or an estimate of missing opponents; it assumes the historical coverage is the relevant population partition and ignores uncertainty in its parts. It cannot establish a positive whole-field effect.

Zero recorded illegal actions or timeouts means no such events were found in those completed evaluation rows. It does not establish a zero underlying failure probability, safety on arbitrary inputs or a guarantee about longer games and finalist timing. The training rollout code assigns an incomplete game a -0.25 reward; completed win/draw/loss rewards are +1/0/-1. The final evaluation contains only completed games, so that incomplete-game value does not enter the quoted +0.2686 result. Training's terminal-only label does not erase this failure convention.

Discounting applies between recorded policy decisions, not uniformly between real-world seconds or full turns. A gamma of 0.997 therefore weights terminal consequences according to decision distance; regularization also affects optimization. Those are reasons to evaluate actual complete-game outcomes and useful alternative proposals, rather than presenting the training objective as identical to an undiscounted competition win rate. The present records do not isolate the consequences of that design choice.

## Why the paired records matter

For candidate outcome X and comparator outcome Y, the sample variance of the paired difference satisfies `Var(X - Y) = Var(X) + Var(Y) - 2 Cov(X, Y)`. On the retained 5,000 Lucario RL pairs, covariance is 0.332923 and the paired standard error is 0.0154938 reward units. Discarding the pairing and imposing independence would give 0.0193191. This is a same-record explanatory calculation, not an alternative primary test or additional experiment. Pairing need not reduce variance in every problem: its effect depends on the covariance under the chosen scenario design.

In the hybrid confirmation, each pair ran the candidate and its parent through the local deterministic engine bridge with one engine seed, one opponent program and one simulator slot, with slots alternating by pair index. The seed matches starting conditions under this bridge; different actions can change later random-number consumption and trajectories. Later events may therefore diverge, but not every changed action necessarily changes every later draw. The retained rows record the two outcomes, not the point of divergence. The engine offers the go-first choice to slot 0 at setup, and our rule program always accepts it, so in slot 0 our program took the first turn while in slot 1 the opponent program decided; a slot fixes who answers that question, not turn order in general. The later RL evaluation likewise records one engine seed and one policy seed per scenario for both arms, and the same reading applies to it.

For the historical hybrid summary, each opponent still contributes fifty pairs. Historical weighting changes each row's contribution to the mean and uncertainty, not how many games were run for that row. The displayed weights sum to one over the covered portion; they do not imply complete population coverage. The corresponding standard error uses squared weights and fixed-stratum paired variances. Neither this calculation nor the recurrent covariance check measures uncertainty in the simulator, the opponent population or earlier design selection.

An average gain also does not mean that each decision improved. The hybrid's 87 better, 35 worse and 378 equal pairs are complete-game outcomes; equal pairs may contain different actions and trajectories. The three historical stages support a repeated local comparison under their described selection history. Their decreasing point estimates alone cannot identify whether selection effects, scenario variation or another mechanism explains the differences. Do not pool the stages into an unqualified larger final test.

## Cross-deck context

A prior separate Crustle/Mega Kangaskhan run improved terminal reward by +0.3928 over its own replay-BC parent in 5,000 locked local pairs (bootstrap 95% interval [+0.3622, +0.4228]). Both player positions and ten opponent groups were positive. Its final-record and checkpoint identities were rechecked separately. This is another deck-specific local result under a related evaluator, not unseen-population transfer, a universal method ranking, or a causal test of hardware quantity.
