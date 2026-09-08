# Strategy writeup companion

Alessandro Flati and Roberto Mastropietro

An optional reference and evaluation guide for our [Kaggle Strategy submission](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle-challenge-strategy/writeups/new-writeup-1783092659035).

The submission link is access-dependent: our unauthenticated check returned 404. The submission was verified in our account; this repository does not change its visibility. The [competition page](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle-challenge-strategy) provides the platform's current entry point.

Start with the writeup: it contains the complete main report. This optional companion helps readers locate original sources and check the arithmetic of its existing local evaluation. It is not an agent release or a runnable reproduction package; no assumption is made about whether judges will consult it.

## Find the relevant material

| Reader question | Where to look |
|---|---|
| Which public baselines does the writeup acknowledge? | [Public baseline references](REFERENCES.md#public-baselines) |
| What is the background for attention and replay imitation? | [Method references](REFERENCES.md#method-background) |
| Can I recalculate the reported improvement and uncertainty? | [Aggregate results and calculation](EVALUATION.md#confirmation-aggregates) |
| How do twelve simulated worlds differ from 500 game pairs? | [Comparison units](EVALUATION.md#comparison-unit) |
| Are distributed RL and regret learning part of the submitted agent? | [Further reading and implementation boundaries](REFERENCES.md#further-reading-not-the-submitted-algorithm) |
| Which version does this companion describe? | [Version identity](VERSION.json) |
| Where do data and source permissions come from? | [Scope and rights notice](NOTICE.md) |

## System in brief

The submitted system keeps one Mega Lucario deck. A rule policy proposes an action; an attention network trained to imitate recorded replay choices proposes another. The network scores legal options, not game-winning probabilities. During eligible disagreements, a heuristic model of the opponent must put at least 0.95 of its mass on one of ten reference decks before search runs. That closed set and threshold do not guarantee correct opponent identification.

Search compares the two moves in twelve paired hidden-state completions. After the first move, rule policies continue the simulated games to terminal outcomes. Only a strictly higher mean accepts the learned alternative; agreement, ties, ineligible choices and failed comparisons retain the rule action. The reported complete-game evaluation tests this combined system, not the isolated contribution of each component. Later tests of the unchanged submitted system do not retroactively become official competition results.

### What adapts during play

Neural fitting and checkpoint selection happen offline. The writeup's 270 games and 17,571 decisions describe the full fine-tuning corpus including its splits, not the training split alone. During play, neural weights and the playable deck remain fixed. Revealed cards update the within-game opponent belief, and bookkeeping records actions actually played. This is observation-conditioned decision-making, not online neural training or learning across a best-of-three match.

### What bounds the computation

Two proposals across twelve worlds mean up to 24 branch evaluations per eligible search. Each branch allows up to 400 continuation action steps after the forced first move. A branch without an observed terminal score at that limit is incomplete; missing any required paired result keeps the rule action. The cap limits simulation steps, not elapsed seconds: step cost varies, and the cap is not a wall-clock timeout.

The submitted entrypoint forces CPU inference and processes worlds serially. Faster neural inference and parallel simulator branches are different optimization targets. Which dominates elapsed time needs profiling; this implementation description alone does not establish a GPU speedup or improved playing strength.

## Reading boundaries

- A reference establishes its own method or origin, not our performance.
- Public notebook scores and titles describe their authors' material, not a directly comparable benchmark for our entry.
- The companion does not contain competition data, card assets, replays, copied notebooks, model weights, agent code, or training code.
- The writeup remains understandable without opening this repository. No result depends on a reader executing external resources.
- Linked resources may require Kaggle sign-in or acceptance of the competition rules. They are not mirrored here.

This repository is maintained by the participants; it is not an official organizer resource or an endorsement by any referenced author.
