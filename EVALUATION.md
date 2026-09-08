# Interpreting the local evaluation

This note explains the estimator and scope of results already reported in the linked writeup. It contains no raw game records and cannot independently reproduce the games.

## Comparison unit

A pair consists of two complete local games: the submitted hybrid and its exact rule baseline, using the same playable deck, opponent program, random seed and simulator player slot. A shared seed controls the randomization design; differing actions can still produce different trajectories. Slot 0 or 1 identifies simulator player position, not necessarily who takes the first turn.

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

## Counts, selection and scope

The confirmation panel has 87 pairs favoring the hybrid, 35 favoring the baseline and 378 with equal outcomes. Equal outcomes can include two losses or two wins; they are not necessarily drawn games. These unweighted pair counts should not be used to reconstruct the frequency-weighted effect without opponent-level outcomes and weights.

The development panel informed design selection. The preliminary recheck determined whether confirmation ran. Confirmation used disjoint seeds but retained the same system, simulator, roster and weighting scheme. It therefore strengthens the local comparison without independently validating the opponent population.

The writeup also reports leave-one-opponent-out sensitivity: omit one represented opponent, renormalize the remaining weights and recompute Delta. This reuses the same outcomes; it is not a new replication. Neither that check nor a positive aggregate rules out an adverse matchup or establishes performance against unrepresented opponents.

## What this comparison cannot establish

- A competition win rate or a change in official leaderboard score.
- The causal share attributable separately to imitation, belief updates or search.
- Superiority over every same-deck policy or over all participants.
- Robustness to unknown opponents, arbitrary initial states or a different simulator.
- Measured finalist-hardware acceleration or best-of-three adaptation.

The source and analysis identities in [VERSION.json](VERSION.json) make the retained artifacts distinguishable. Hashes are identity commitments, not public access to the artifacts or proof that an experiment ran. Full gameplay reproduction requires the permitted environment, exact policies, weights and underlying records, which are not distributed here.
