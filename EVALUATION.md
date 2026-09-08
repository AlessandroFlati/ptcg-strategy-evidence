# Interpreting the local evaluation

This note explains the estimator and scope of the local confirmation result reported in the linked writeup. Participant-authored aggregate summaries below let readers recalculate that result and its uncertainty. They contain no raw game records and cannot independently reproduce or authenticate the games.

## Comparison unit

| Quantity | Unit | Question answered |
|---|---|---|
| Twelve paired worlds at an eligible decision | Two proposed moves evaluated under each sampled hidden-state completion | Which proposal scores higher under these sampled states and continuation policies? |
| 500 pairs in confirmation | Two complete games per pair, one for each tested policy | How did the whole hybrid compare with its rule baseline under the local evaluation protocol? |

A worked decision's 8/12 rollout score is not the hybrid's complete-game win rate. Twelve search worlds are internal to action selection; the 500 evaluation pairs are the separate whole-game comparison.

A pair consists of two complete local games: the submitted hybrid and its exact rule baseline, using the same playable deck, opponent program, random seed and simulator player slot. A shared seed controls the randomization design; differing actions can still produce different trajectories. Slot 0 or 1 identifies simulator player position, not necessarily who takes the first turn.

Both policies retain the baseline's replay-trained action priorities and card-specific rules. The hybrid adds the separate neural proposal, opponent-belief gate and terminal comparison. The measured gain therefore concerns these combined additions; it does not isolate the value of all learning against purely hand-written rules.

The confirmation panel contains 500 pairs: 50 per opponent across ten opponent programs, with 25 pairs in each slot per opponent. That is 500 games per policy, or 1,000 games total. These locally implemented opponents represent 78.0% of a historical collected three-day sample; they are not every real competitor or the entire competition population.

## Weighted paired estimator

For opponent j and pair i, define d[j,i] as the hybrid outcome score minus the baseline outcome score. Scores are 1 for a win, 0.5 for a draw and 0 for a loss. Let n[j] be its pair count and s[j] its historical sample share.

- Normalize the represented shares: w[j] = s[j] / sum(s[k]).
- Compute each opponent's mean paired difference, mean(d[j]).
- Weighted difference: Delta = sum(w[j] * mean(d[j])).
- Paired standard error: SE = sqrt(sum(w[j]^2 * sample_variance(d[j]) / n[j])).
- Express Delta and SE in percentage points by multiplying by 100.

The standard-error calculation treats opponent strata as independent and the historical weights as fixed. It does not quantify uncertainty in population shares, simulator fidelity, opponent modeling, or design selection.

The normal-approximation 90% interval uses Delta +/- 1.6448536269514722 * SE. The confirmation calculation uses Delta = 12.5867346939 and SE = 4.0255053039 percentage points before rounding, yielding +5.97 to +19.21. Recomputing from the table's rounded inputs can shift the last displayed digit. This checks the interval arithmetic; it does not independently verify the underlying games.

The 90% lower bound was the interval used in the confirmation acceptance check. For readers preferring a conventional 95% interval, the same normal approximation gives +4.70 to +20.48 points. This supplementary calculation uses the same games and fixed-weight assumptions; neither interval adjusts for prior design selection.

## Counts, selection and scope

The confirmation panel has 87 pairs favoring the hybrid, 35 favoring the baseline and 378 with equal outcomes. Equal outcomes can include two losses or two wins; they are not necessarily drawn games. These unweighted totals alone cannot reconstruct the frequency-weighted effect; use the opponent-level aggregates and weights below.

The development panel informed design selection. The preliminary recheck determined whether confirmation ran. Confirmation used disjoint seeds but retained the same system, simulator, roster and weighting scheme. It therefore strengthens the local comparison without independently validating the opponent population.

The writeup also reports leave-one-opponent-out sensitivity: omit one represented opponent, renormalize the remaining weights and recompute Delta. This reuses the same outcomes; it is not a new replication. Neither that check nor a positive aggregate rules out an adverse matchup or establishes performance against unrepresented opponents.

## Confirmation aggregates

Every row has 50 pairs, with 25 in each simulator player slot. There were no draws in either policy's 500 games. Wins refer to complete games; better/worse/same compares the two outcomes within each pair. Deck labels identify our local opponent programs, not individual competitors or verified replicas of their policies. The weights are fractions normalized over the represented historical sample, rounded to twelve decimal places.

| Local opponent | Weight w | Hybrid wins / 50 | Baseline wins / 50 | Better / worse / same |
|---|---:|---:|---:|---:|
| Alakazam/Dunsparce | 0.212034813926 | 35 | 32 | 8 / 5 / 37 |
| Archaludon Metal | 0.000600240096 | 26 | 23 | 5 / 2 / 43 |
| Cynthia's Garchomp ex | 0.031812725090 | 35 | 30 | 12 / 7 / 31 |
| Dragapult ex | 0.082683073229 | 39 | 32 | 11 / 4 / 35 |
| Dudunsparce / Mega Lopunny ex | 0.082683073229 | 47 | 35 | 12 / 0 / 38 |
| Great Tusk / Crustle | 0.111344537815 | 39 | 28 | 15 / 4 / 31 |
| Marnie's Grimmsnarl | 0.415816326531 | 28 | 22 | 12 / 6 / 32 |
| Mega Lucario ex | 0.040666266507 | 38 | 34 | 9 / 5 / 36 |
| Mega Starmie | 0.004801920768 | 46 | 48 | 0 / 2 / 48 |
| Team Rocket Mewtwo | 0.017557022809 | 49 | 46 | 3 / 0 / 47 |

For each row, let b and l be the better and worse counts. Because these games have no draws, mean(d) = (b - l) / 50 and sample_variance(d) = (b + l - 50 * mean(d)^2) / 49. Substitute these and w into the estimator above to recover +12.5867346939 points and SE 4.0255053039, up to the printed weights' rounding. For example, Alakazam/Dunsparce has mean(d) = (8 - 5) / 50 = 0.06 and sample variance (13 - 50 * 0.06^2) / 49. This count shortcut would need modification if paired differences could include half-points from draws.

Weighting each policy's wins/50 gives 68.55% for the hybrid and 55.96% for the baseline. Simply pooling wins gives 382/500 = 76.4% and 330/500 = 66.0%, with a +10.4-point difference. The two summaries answer different questions: historical represented-sample weighting versus an equal mixture of the ten tested programs. The historical weighting was retained across test stages, not chosen from this panel's outcomes.

The small Archaludon and Mega Starmie weights explain why their rows have little influence on this weighted estimate; they do not make these matchups unimportant in a different field. Mega Starmie's observed row effect is -4 points. Leave-one-opponent-out estimates range from +11.41 to +14.36 points after renormalization. They cannot address the unrepresented 22.0% or errors in the local opponent models.

As a separate post-panel diagnostic, weighted gains were +17.10 points in slot 0 and +8.07 in slot 1. The slot-0-minus-slot-1 contrast was +9.03 points (SE 7.98; normal-approximation 90% interval -4.10 to +22.16). Positive point estimates in both slots do not establish equal performance across slots, actual turn orders, or arbitrary initial states. These slot summaries use the retained per-slot records, not the pooled table above.

## What this comparison cannot establish

- A competition win rate or a change in official leaderboard score.
- The causal share attributable separately to imitation, belief updates or search.
- Superiority over every same-deck policy or over all participants.
- Robustness to unknown opponents, arbitrary initial states or a different simulator.
- Measured finalist-hardware acceleration or best-of-three adaptation.

The source and analysis identities in [VERSION.json](VERSION.json) make the retained artifacts distinguishable. Hashes are identity commitments, not public access to the artifacts or proof that an experiment ran. Full gameplay reproduction requires the permitted environment, exact policies, weights and underlying records, which are not distributed here.
