# Prospective resource and validation programme

The announced finalist environment lists an H100 80 GB, 16 vCPUs, 256 GiB RAM and a 30-minute per-game budget. Best-of-three (BO3) games are sequential and earlier-game logs are available. Staff says one code/deck combination must remain fixed throughout the round. [Official announcement and replies](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle/discussion/732331)

These are resource and interface premises, not observed speedups or training-access guarantees. On 4 September the organizers [published a card list and future-environment announcement](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle/discussion/739417), stating that the list is planned for both the Playground and the Second Round, and announced a Playground launch for 28 September; a published list identifies cards, not engine support or opponent behavior, so the package-validation step below stands. The submitted agent currently forces CPU neural inference and serial terminal worlds. More GPU memory does not by itself parallelize a native simulator. Pre-tournament training, finalist inference and in-match observation updates are separate activities.

## Ordered experiment contracts

These are proposed contracts, not retrospectively declared criteria for completed experiments. Freeze exact samples, confidence method and operational margins before collecting outcomes.

| Priority | Intervention | Comparison and measurement | Accept / reject logic |
|---|---|---|---|
| 1 | Profile exact baseline and repair feature semantics | Measure initialization, encoding, inference, simulator continuation, p50/p95/max and cumulative time; regenerate damage/knockout (KO) features consistently | Reject parity or legality drift before any claimed acceleration; behavior-changing fixes need their own game comparison |
| 2 | Isolated parallel world workers | Same twelve worlds/actions/seeds versus serial execution, including errors and state counters | Require exact intended parity and improved end-to-end tail time; reject unsafe shared state or overhead that removes the benefit |
| 3 | GPU inference / batching | CPU and GPU with the same checkpoint, legal menu and dtype; include warmup and transfer overhead | Require action parity where semantics should be unchanged; report speed only under measured batch/menu sizes |
| 4 | Reinforcement-learning (RL) proposals | Existing versus RL proposal with search fixed, plus rules-only, imitation-only and recurrent-RL-only arms | Use a separate development bank, then a locked representative panel against a qualified same-deck comparator; require a positive paired interval and pre-set adverse-matchup/safety criteria |
| 5 | Adaptive search budget | Fixed worlds versus confidence/disagreement-based allocation at equal total wall time | Reject if additional compute only increases latency, harms important groups or compensates for a weaker comparator |
| 6 | Broader opponents and new cards | Validate actual finalist package; hold out opponent policies and audit revealed-card beliefs | Reject unsupported card/API assumptions; wider sampling cannot repair a misspecified opponent model |
| 7 | BO3 log-conditioned decisions | One fixed program/deck with versus without earlier-game public logs | Check log schema, persistence, cross-match reset, leakage and cumulative per-game time; require paired match-level benefit over the memoryless control |
| 8 | Further training / regret research | Independently available development resources; compute-matched controls and expanded action coverage | Deep Counterfactual Regret Minimization (Deep CFR) first needs coverage/objective-alignment tests. Do not interpret more episodes or richer labels as playing strength |

The historical project contains measured parallel acquisition work, but those measurements concern a particular data-generation task. They do not predict H100 inference speed or show that a larger policy collection can be selected effectively during play. The most direct near-term opportunity is to measure whether stronger learned alternatives improve the existing comparison mechanism.

Public results also justify a search-free control: the [15th-place author](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle/discussion/739241) reports retaining direct learned action selection after additional inference-time variants underperformed. This observation is local to that system. In our experiments, retain recurrent-RL-only alongside the RL-proposal hybrid; if direct play is stronger at the matched budget, keep it. Additional compute is useful only insofar as it improves the tested deck-policy combination, not because it preserves our current architecture.

## A resource budget that survives a complete game

The announced thirty minutes is cumulative thinking time for each game; unused time cannot be transferred to the next game. A per-decision latency screen therefore cannot certify finalist feasibility. Measure startup plus all decisions, include difficult long-game cases, and retain a prospective safety reserve for future turns. When a comparison cannot finish within that reserve, execute the accepted fallback. This is a proposed controller, not an existing measured property of the submitted archive.

Assign CPU workers to isolated simulator continuations and the GPU to measured neural batches. Treat the RAM allocation as capacity to measure per-worker memory and avoid resource contention, not evidence that sixteen workers will scale linearly. Benchmark the full path, including transfers and warmup, before expanding search. Training and in-game inference still require separate access assumptions.

Record the exact starting state, proposed moves, sampled completions, Python seeds and native-engine seeding mode in parity tests. Check outcomes as well as actions: a port can preserve a root move while changing the simulated evidence behind it. The local deterministic bridge is a measurement device, not proof of identical deployed native randomness. Separate behavior-preserving engineering from changes to features, proposal history, hidden-state inference or continuation policies; the latter require complete-game strength tests even when latency improves.

## Make the next result answer one question

The most informative first learning experiment replaces only the proposal source: use the same eligible states, opponent-belief rule, hidden-world procedure and continuation policies. Record proposal legality, agreement with the baseline, how often search changes the chosen move, and paired complete-game results. A learner can improve standalone results yet supply no useful alternatives to this arbiter; that outcome would reject this integration even if its original RL comparison remains positive.

Then compare fixed-world and adaptive allocation policies at matched wall time. Repeatedly checking an ordinary fixed-sample confidence interval and stopping when it becomes positive does not preserve its advertised coverage. Freeze a valid sequential rule or fixed checkpoints with an appropriate error-control plan before collecting outcomes. [Howard et al., confidence sequences](https://arxiv.org/abs/1810.08240) provide relevant statistical methods; their assumptions must be checked for the chosen paired rollout process, rather than importing a guarantee by name. Test realized game strength and tail time as well as rollout estimates. More accurate estimates under a misspecified hidden-state or continuation model need not improve decisions.

If search expands beyond the existing root comparison, test whether continuation choices improperly use unrevealed information from a sampled world. A [participant's search-API investigation](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle/discussion/711329) illustrates this risk around successive draws. Action selection must respect the information available at that decision and preserve genuinely known deck-order constraints. More worlds or arbitrary reshuffling alone does not establish information-set-correct planning. This is a prospective correctness test, not a claim that our present rollouts implement full imperfect-information search.

Finally, earlier-game log conditioning needs a separate BO3 interface and match-level experiment. The existing recurrent learner's own-action buffer resets per game and does not implement this. Freeze one whole-round code/deck, update only permitted observation-derived state, and compare against the same program with cross-game information withheld. An observed learning gain, a faster simulator and a BO3 information gain are different results and must remain separately attributable.

The proposed sequential search design must also specify its sampling assumptions. The current systematic reference sampler does not produce independent and identically distributed (IID) reference draws. A time-uniform interval cannot be pasted onto that process without checking the relevant conditional-mean and dependence conditions. Start with a fixed, reproducible budget comparison if the sequential assumptions are not established; do not certify adaptive stopping through repeated ordinary confidence intervals. The cited confidence-sequence research supplies methods to assess, not an already integrated guarantee.

## Experiments that isolate the proposed improvement

These are prospective designs, not additional results. Fix budgets, opponents, seed banks and acceptance rules before running them.

| Arm | Proposal source | Terminal comparison | Primary interpretation |
|---|---|---|---|
| Rule control | Existing rule/priority program | Off | Accepted same-deck reference |
| Imitation-only | Existing replay-trained model | Off | Standalone learned policy under specified fallback/decoding |
| Current hybrid | Existing replay-trained model | Existing settings | Reproduces the integration reference |
| RL-only | Separately trained recurrent model | Off | Standalone later learner on the common panel |
| RL-proposal hybrid | Recurrent model on eligible single-choice states | Same arbiter as current hybrid | Isolates replacing the proposal source within that integration |

The primary integration contrast is RL-proposal hybrid minus current hybrid. The two standalone learned arms ask a different question. Define the recurrent history update on every real action, including those selected by the rule program, before testing; otherwise the proposed hybrid changes the learner's input semantics. Holding the arbiter fixed does not guarantee the two complete-game trajectories visit identical states. A separate matched-state diagnostic can inspect proposal differences, but complete-game comparison remains necessary. No result should be obtained by adding the standalone RL gain to the current hybrid gain.

Opponent validation must hold out executable policies as well as deck labels. Reserve implementations or styles not used in training or selection; check card legality, representative behaviors, both player slots and the same engine version. A new policy using a known deck can expose a different weakness. Keep the historical panel for continuity, but report the holdout separately instead of changing its weights after looking at outcomes.

Card-ratio testing needs two stages. First, replace a specified small set of counts while holding the executable policy fixed and keeping every list legal; this measures that deck change under that policy. Then, if adaptation or retraining is intended, allocate matched development budgets to both deck-policy combinations and test on new seeds. Predeclare the intended resource/knockout tradeoff, retain unsuccessful lists, and reject a change that misses strength or adverse-matchup limits. A successful predicate ablation cannot substitute for this deck-construction evidence.

For opponent belief, reserve games with known hidden deck lists for evaluation only. At selected public prefixes, compare peak belief with actual reference compatibility, include deliberately out-of-roster decks, and record how often the confidence gate opens on an incompatible list. Use only information available at each prefix to construct the belief. Check filler counts and simulated legality separately. A confidently wrong gate or persistent hidden-card inconsistencies would reject expansion even if in-roster identification looks good.

For BO3, compare the same fixed code with authentic earlier-game public logs, a no-cross-game-information control, and a diagnostic control receiving plausible but unrelated public logs under matched processing budgets. Synthetic or mismatched logs are test controls, never presented as recorded match evidence. Verify that no later-game result enters an earlier decision and that state resets between matches. Report match wins and game/slot breakdowns; a pooled single-game improvement does not by itself establish a BO3 benefit. Where a control changes visited states or total time, preserve that distinction rather than crediting information alone.

## Diagnose the proposal experiment before scaling it

The first integration should return exactly one legal alternative on the existing search interface. The recurrent decoder must map its canonical index back to the engine menu, and its history must record actual executed actions, including rule-selected actions. Keep the current terminal continuation policies for this comparison. Using the recurrent learner inside continuations as well would introduce another intervention. The learner's value head is not yet a replacement for terminal outcomes.

| Measurement | Denominator or comparison | Decision it can support |
|---|---|---|
| Interface eligibility | Qualifying single-choice decisions / all controlled decisions | Shows how much of actual play the proposed integration can reach |
| Legal disagreement | Legal alternatives differing from rules / eligible decisions | Shows whether the new proposer supplies different candidates; report invalid outputs separately |
| Search completion | Complete comparisons / comparisons opened after the belief gate | Exposes continuation limits and compute loss |
| Executed override | Accepted alternatives / complete comparisons, and separately / all decisions | Shows how often search changes play; higher frequency is not inherently better |
| Strength and cost | Paired complete-game outcomes and cumulative clock against the current hybrid | Determines adoption under locked strength, adverse-matchup and timing criteria |

Do not compare conditional rates with different denominators, or interpret increased disagreement as increased strength. Report the belief-gate pass count between disagreement and search opening. A useful standalone learner might agree with rules at eligible states, differ mainly on multi-selection states outside this interface, or propose moves the current arbiter rejects. Those explanations call for different experiments; they do not justify assuming that its standalone gain transfers.

Matched-state diagnostics answer where proposals differ on a chosen state bank. Comparing wins only in games where an override occurred conditions on events produced by the tested policy and can select different kinds of games. It does not isolate the override's causal benefit. Keep the complete-game primary comparison, and label any intervention-conditioned summaries as descriptive.

## Preserve the measurement when implementation changes

Before a CPU/GPU or batching comparison, fix dtype, checkpoint, feature schema, canonical-to-engine mapping and tie-breaking. Specify a numerical tolerance on scores and separately count action disagreements, especially near ties. Passing a tolerance does not excuse a changed move. If numerical differences change behavior, treat the port as a candidate policy requiring game tests; report its speed and behavior changes separately. A feature-schema hash detects a version mismatch, not whether the encoded features are strategically correct or complete.

Count every scheduled scenario in the experiment manifest. Report completed pairs, missing/failed pairs, timeouts and retries, and predefine their treatment before looking at outcomes. A complete-case win rate can conceal failures correlated with difficult games. Preserve both arms when a paired rerun is necessary; do not selectively replace an unfavorable outcome. The retained final evaluation has all 5,000 pairs complete; this is a prospective failure-accounting contract, not an allegation that its reported gain omitted failed pairs.

For development learning, profile simulator throughput, learner updates and queue/policy lag separately. More actors can increase both experience throughput and policy staleness. Track behavior versions, importance-ratio summaries and clipping alongside fixed-budget game performance; raw episode count alone cannot establish useful learning. These are proposed measurements, not a speed forecast for the H100 or a claim that queue lag caused this run's limitations.

## Why BO3 needs match-level evidence

Even without adaptation, a pooled single-game win percentage does not determine best-of-three performance. In a deliberately simplified no-draw model with independent games and constant win probability p, the match-win probability is `3*p*p - 2*p*p*p`. If instead all three potential outcomes share one win/loss outcome, the same per-game marginal p yields match probability p. At an illustrative p of 0.6, those are 0.648 and 0.6. Neither example is a model fit to our games.

Real match structure, shared opponents, draws, player-position rules and earlier-game adaptation require the actual supplied interface and match-level evaluation. Do not convert the retained 45.68% local single-game result into a predicted BO3 score. Keep match wins as the primary BO3 outcome and record game-level results as supporting diagnostics.

## Test deck changes together with their policy consequences

If additional compute is allocated to deck construction, first declare whether the experiment tests a card substitution under the unchanged policy or a jointly retuned deck-policy combination. Both are useful questions, but they estimate different effects. Changing an Energy count can change Lunatone access, attachment availability and Aura Jab's discard supply; a raw-card access calculation does not capture those competing consequences.

Develop substitutions on separate scenarios and preserve the final comparison panel before selection. Keep each candidate legal, declare the number of candidates screened and compare the selected pair against the retained sixty-card combination with matched complete games. Report adverse matchups and resource/runtime costs, not only the best development result. Where feasible, include both unchanged-policy and retuned-policy arms so a card's apparent gain is not silently credited with the benefit of extra policy work. More compute permits broader tests; it does not establish that a better deck ratio exists or will be found.

## References and limits

- [Existing public baseline and attention/imitation references](https://github.com/AlessandroFlati/ptcg-strategy-evidence/blob/main/REFERENCES.md).
- [IMPALA / V-trace](https://arxiv.org/abs/1802.01561): method background for the separate recurrent learner; no imported Atari/DMLab performance claim.
- [Deep Counterfactual Regret Minimization](https://arxiv.org/abs/1811.00164): reference method, not a claim that our limited branch realizes the paper's full algorithm or guarantees.
- [Permission for additional analysis](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle-challenge-strategy/discussion/735656): later results remain separate from the ranked submission.
- [Current media guidance](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle-challenge-strategy/discussion/736603): supporting materials do not change the ownership of supplied elements.

The [Japanese translation](WRITEUP_JA.md) is a reading aid, not a judging criterion or evidence that particular judges prefer a language. It corresponds to the English source identified in [VERSION.json](VERSION.json), preserves numerical and evidential qualifiers, and discloses its AI review and lack of native-language review. This companion contains no measured finalist-hardware result.
