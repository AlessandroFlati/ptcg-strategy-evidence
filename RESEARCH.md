# Questions, interventions and decisions

## Our contribution

The submitted system connects a deck-specific rule/priority policy, a learned legal-option proposal and a terminal-game comparison. Three implementation choices matter:

- Source/target-aware option tokens allow different uses of one card to be represented distinctly. State and option tokens interact through attention; the legal menu can change size.
- Search is conditional on an eligible disagreement and the closed reference-deck belief threshold. It compares the two actions under matched hidden-state samples, rather than accepting an imitation score as game value.
- An override reconciles resource-use counters. The next decision must reflect what was actually played, not what a discarded proposal would have consumed.

The contribution is the coupled implementation and evidence for its decisions. Attention, imitation, hidden-state sampling and paired evaluation are established methods. Public baseline origins remain acknowledged in the report and reference guide.

The design divides two different questions: whether a move resembles recorded play, and whether it improves simulated consequences relative to the current choice. The first supplies candidates; the second arbitrates between them. Restricting this comparison to disagreements bounds the work and leaves agreement decisions untouched. Terminal continuations can assign outcome estimates to setup choices such as drawing instead of fetching a Pokemon, but they inherit the reference decks and continuation policies' errors. This is design rationale, not an isolated component-effect estimate.

### Public context and the specific integration claim

Public reports include [gated replay rankers](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle/discussion/738633), [gated BC/PPO specialists](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle/discussion/739219), and [extensive controlled negative-result analysis](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle/discussion/724187). Using a gate, semantic features or falsifiable tests is not unprecedented. Our explanation instead connects three inspected mechanisms to a complete-game comparison:

| Mechanism | Concrete role in this submission | Evidence boundary |
|---|---|---|
| Source/target-aware options | Distinguish legal uses of the same card before ranking alternatives | Representation and bounded fixtures; no isolated strength attribution |
| Two-proposal terminal comparison | Compare the rule choice with a learned disagreement under the specified hidden-state approximation | Worked trace and combined-policy evaluation; other root actions remain unsearched |
| Post-override state reconciliation | Correct resource counters when the executed move differs from the proposed rule move | Inspected code and matched instrumented replay; not proof of every mutable-state path |

The hypothesis/test/decision account explains why a particular executable was retained. Its value is inspectable reasoning and outcomes, rather than the number of methods attempted or claims that each component independently improved play.

## Deck strategy: access, timing and competing resources

The deck creates a useful resource cycle, but each link has conditions. Lunatone turns one Basic Fighting Energy in hand into three drawn cards when Solrock is in play. Lunar Cycle is limited to once per turn across all copies: two Lunatone do not imply six cards drawn by repeating that ability. Aura Jab then makes discarded Energy useful again by attaching up to three to Benched Pokemon. This prepares later attackers only when Energy is available in discard and recipients are on the Bench; the attack does not accelerate its own Active attacker.

Alternative uses of a resource explain why fixed priorities can disagree with a learned proposal. Discarding Energy may enable later acceleration, but Energy retained in hand is still needed for manual attachment and future draw costs. Discarding two other cards for Ultra Ball obtains a specific Pokemon at a hand-development cost. A knockout with Mega Brave may be worth giving up Aura Jab's preparation, but damage alone does not measure the next board's quality. These are strategic tradeoffs implied by the mechanics, not separately measured causal gains.

### Search cards do not provide interchangeable access

| Desired card in our submitted deck | Fighting Gong | Poke Pad | Ultra Ball |
|---|---|---|---|
| Makuhita, Lunatone, Solrock or Riolu: Basic Fighting Pokemon | Yes | Yes | Yes |
| Hariyama: Stage 1 without a Rule Box | No | Yes | Yes |
| Mega Lucario: Stage 1 with a Rule Box | No | No | Yes |
| Basic Fighting Energy | Yes | No | No |

The twelve search Items are therefore not twelve equivalent ways to obtain Mega Lucario. Ultra Ball also requires two other hand cards to discard; a legal target must remain in the deck. Search access is not the same as drawing the target, putting it into play or meeting evolution timing. This table explains functional coverage, not the probability of a complete opening setup or proof that four copies of each Item are optimal.

### Timing and recovery have costs

Hariyama's gust, which moves an opposing Benched Pokemon into the Active Spot, is triggered once when it is played from hand to evolve, rather than being repeatable on every later turn. Its Wild Press costs three Fighting Energy, deals 210 damage and also deals 70 to itself. The normally one-Prize alternative therefore has setup, Energy and durability costs. Solrock's Cosmic Beam has a different partner condition from Lunar Cycle: Lunatone must be on the Bench, whereas Lunar Cycle requires Solrock in play. Position matters as well as card presence.

Wally's Compassion heals a Mega Evolution ex and returns all its attached Energy to hand only if damage was healed. It does not clear Mega Brave's restriction. Recovery can preserve an attacker while removing its immediate Energy supply; it should not be described as a free reset or a guaranteed tempo gain. Lillie's Determination draws eight only with six Prizes remaining, otherwise six. Judge gives both players four cards, so its hand renewal and disruption value depend on both hands rather than on a universal preference over Lillie's.

The low-deck rule is narrower than a ban on drawing: at ten or fewer cards, it replaces a chosen Lunatone ability only if a different ranked alternative exists. Keeping the ability when no alternative exists is part of the inspected implementation. This is an explicit policy decision; its description does not establish the optimal threshold. Likewise, Cape's additional HP is a mechanic, not an isolated effectiveness result.

### What the evidence supports

Card text supports these interaction and access explanations. The selected replay supports one actual Ultra Ball-versus-Lillie's decision and the behavior of its comparison procedure. The knockout-rule ablation supports one policy predicate under its stated opponent protocol. The complete-game hybrid comparison supports the combined executable with the fixed deck. None of those alone establishes optimal card ratios, the frequency of every described interaction, or each card's contribution to the aggregate gain.

The knockout-rule ablation's comparator needs one identity note. The retained lab record removed the Premium Power Pro stacking predicate from the rule program after its 7 August Full Metal Lab scope correction, using the hybrid lab's paired row gate, which reuses the confirmation instrument's pairing code with seat-alternating shared seeds, against the Archaludon proxy: 4,800 pairs in six disjoint seed blocks, +3.833 points for keeping the predicate, paired standard error (SE) 0.574, all six blocks positive. The archived submitted parent lists both that correction and the predicate among its included hypotheses, so "corrected" identifies the submitted parent's lineage after that correction rather than a different policy family. The ablation build and the 16 August archive are not shown to be byte-identical, and the same record attributes roughly two thirds of the predicate's Archaludon-row value to the correction itself. That is why the main report scopes the result to one predicate under one opponent protocol.

A damage-immunity spending veto does not imply that Aura Jab's Energy-acceleration effect has no value into an immune target. Retreating and returning can clear Mega Brave's restriction only when the necessary actions and resources are available. These conditions preclude turning the role descriptions into universal play recommendations.

Card mechanics were checked against the retained competition card-data rows for the seventeen submitted IDs. The two English copies agree for those rows after normalizing a column-header typo and line breaks, although other cards differ between the full files. This is local source consistency, not a claim that every version of the data or simulator is identical. Original data access is through the [Strategy competition data page](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle-challenge-strategy/data); platform access may be required.

## Hypothesis and decision evidence

| Question | Intervention / falsifier | Evidence | Consequence and scope |
|---|---|---|---|
| Does the combined policy improve the exact baseline? | Complete-game paired comparison with the same deck, opponent, seed and simulator slot | Final confirmation: 500 pairs; weighted gain +12.587 points, SE 4.026 | Supports the combination locally; no isolated learning/search effect |
| Does broader search earn its runtime? | Pre-set internal p95/max criteria of 12.5/20 seconds, alongside an 80-pair development screen | Favorable screen; p95 13.71 and max 46.73 seconds | Rejected before submission; a selected screen does not prove a globally stronger candidate |
| Does richer imitation deserve gameplay testing? | Overall action-set accuracy and a predeclared 3.0-point subgroup-loss limit | +2.36 points overall; -3.046 on Dragapult | Rejected before gameplay testing; exceeding the limit by 0.046 points enforces the decision rule, but does not establish a statistically meaningful accuracy loss or a playing-strength loss |
| Does a knockout predicate change outcomes? | Remove the corrected baseline's Premium Power Pro knockout rule | 4,800 balanced Archaludon pairs; retained rule +3.833 points, SE 0.574 | Supports that predicate under this opponent protocol; does not prove optimal card counts |
| Can recurrent reinforcement learning (RL) improve replay imitation? | Separate checkpoint selection and locked complete-game comparison | Lucario: 5,000 pairs; mean terminal-reward gain +0.2686 | Local learning evidence against replay behavioral cloning (BC), not an improved official result or demonstrated hybrid upgrade |

The examples were selected retrospectively for explanation. Prospective criteria are described only where retained protocols establish them. Later confirmation is not retroactively part of pre-submission selection. A later negative wider-search check survives only as decision metadata, without reconstructible rows; it is not presented as a reproducible numerical result here.

## Research breadth without conflating methods

| Research direction | Strategic question | Retained lesson | Status in the submitted hybrid |
|---|---|---|---|
| Replay imitation | Can recorded choices propose useful alternatives? | Agreement labels are useful proposals, not certified optimal actions | Integrated proposal source |
| Terminal search | Which proposal has better simulated consequences? | Pair sampled conditions and retain a defined fallback | Integrated, bounded to two root actions |
| Opponent belief | When is reference-based simulation worth invoking? | A concentrated closed-set belief can gate search but does not identify arbitrary opponents reliably | Integrated ten-reference gate |
| Learned value / offline Q | Does outcome prediction rank competing actions? | Prediction on logged actions did not establish useful sibling-action discrimination | Not integrated |
| Policy selection | Can public observations select the strongest policy for a state? | Hindsight oracle headroom did not yield a useful deployable selector | Not integrated |
| Regret learning | Can strategic alternatives be improved through regret updates? | Our branch's updates covered only ATTACK/RETREAT/END; historical failure causes remain partly unresolved | Not integrated; not a ready-made Deep Counterfactual Regret Minimization (Deep CFR) upgrade |
| Recurrent RL | Can replay initialization be improved by complete-game training? | Separate post-submission experiments improve on their own replay-BC baselines | Not integrated into the submitted hybrid |
| Deck-policy evaluation | Which fixed combination should be retained? | Deck and executable policy must be evaluated together | One final playable deck, not runtime deck switching |

Breadth does not establish that every branch influenced the original design or was scientifically successful. Its value here is that it exposes distinct questions and the evidence needed before combining methods.

Two distinctions explain the rejected learning directions. Predicting the outcome after the action that was actually logged does not establish how a different action in that state would rank. Similarly, choosing the best policy retrospectively after seeing all outcomes measures an oracle's opportunity, not whether a selector can identify that policy from the information available during play. These are different missing tests, rather than evidence that all learning methods failed for one hardware-related reason.

A [public 492nd-place solution](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle/discussion/739956) describes behavioral cloning followed by proximal policy optimization. This supports treating BC-to-RL as established practice. Our separate learner uses recurrent own-action history and off-policy corrected updates; it was not integrated into the submitted hybrid.

The [15th-place public methodology](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle/discussion/739241) reports a 7.5M recurrent actor-critic, distributed population self-play and no behavior cloning, demonstrations, runtime search or handwritten overrides. Its lineage began randomly; Slowking later warm-started from Dragapult. Terminal game results supplied strategic reward, while auxiliary objectives supplied other learning signals. Older-policy corrections are described but not named V-trace. The reported 5.5 billion decisions and roughly five RTX 4090 GPU-days are not a scaling experiment; GPU-days were estimated from throughput, not logged exactly.

That account also reports worse results for its tested extra inference-time evaluation than direct action selection. It motivates keeping search-free learned play as a serious control, not concluding that search hurts our different policy. The public evidence supports multiple successful designs. Neither our local gains nor comparisons between unlike reported budgets establish a method ranking or a hardware-only explanation for our lower official result.

## Worked decision provenance

The primary example is a selected local replay of the retained submitted archive, with ordinary and read-only instrumented runs matching actions, public observations and bookkeeping. At the selected state, the three-card hand and six Prizes make Ultra Ball's cost and Lillie's eight-card draw consequential. Twelve matched terminal samples score them 0/12 and 8/12. A later Mega Brave/retreat tie at 10/12 retains the baseline.

These are mechanism examples. They do not establish how often the learner overrides, a general preference for Lillie's, an official-match win, or twelve independent complete-game evaluation wins. Gallery diagrams show only these recorded facts, without fictional hidden hands or recreated card artwork.

## Exact boundaries of the submitted decision rule

The following describes the inspected submitted archive, not a proposed generalized search agent. The rule/priority program produces a choice first. Search handles only a one-option choice with at least two available options and a selection interface that allows at most one. It does not replace the rule program's multi-card selection decisions.

| Condition | Submitted behavior | Interpretation |
|---|---|---|
| One available option, multiple required selections, or no single rule proposal | Keep the rule proposal | No terminal comparison is opened |
| Alternative agrees, or is not a single-option choice | Keep the rule proposal | A larger model does not automatically increase search frequency |
| Opponent belief peak below 0.95 | Keep the rule proposal | The model declines this comparison; this is not evidence the unknown matchup is safe |
| A sampled pair does not produce both terminal outcomes within the continuation cap | Reject the incomplete comparison and retain the rule proposal | All twelve paired worlds are required by the submitted setting |
| Twelve complete paired worlds, with the alternative mean strictly higher | Return the alternative and reconcile resource counters | The acceptance rule uses a mean, not a confidence interval |
| Twelve complete pairs with a tie or worse alternative mean | Keep the rule proposal | There is no learned override on an exact mean tie |

The continuation cap is 400 loop steps after forcing the first action. An unfinished continuation returns no value; it is not assigned a guessed draw or value-network estimate. There is no per-decision statistical significance threshold. Twelve worlds are a fixed computation setting, not evidence of converged search or a proved safe policy-improvement rule.

Failure handling has distinct layers. A neural scoring exception returns the earliest legal option indices; that fallback can still disagree with the rule program and enter comparison. Recoverable branch failures yield missing terminal values, which prevent acceptance. Exceptions outside those guarded paths are not guaranteed to return the rule action. The eligibility predicate checks selection shape, not arbitrary index validity. The report therefore does not claim universal exception safety or that every alternative came from a successful neural inference. These are implementation limitations, not observed failure frequencies in the competition.

## Opponent belief and hidden-state approximation

The reducer remembers publicly revealed opponent cards by serial identifier, including cards no longer on the visible board. Reference weights start from the retained historical prior. Reveals update smoothed, card-type-weighted log scores, with soft penalties for excess copies; the result is normalized across the ten references. Normalization always distributes mass within that set. It cannot establish that the real opponent is represented or that a reference policy behaves like that opponent. The ten reference programs are the same ten programs used as local evaluation opponents, so every local evaluation game is in-roster by construction; the gate's behavior against opponents outside the roster is untested by those games.

The world sampler uses systematic weighted selection of reference indices, followed by shuffling. The twelve reference draws therefore should not be described as independent identically distributed deck samples. Each selected list supplies hidden-card guesses. Our visible hand, discard and board are removed from our own list before splitting remaining cards between deck and Prizes. For the opponent, the filler removes currently visible discard and board cards before filling deck, hand and Prizes. Persistent reveal history influences reference selection, but the hidden-zone filler does not implement every inference that could be drawn from that history.

If the available pool is too short, the implementation pads it with the reference list's most common card, with a generic fallback for an empty list. This makes the requested hidden-zone sizes constructible; it does not establish a fully consistent hidden state. More worlds can repeatedly sample the same approximation. A future validator should record incompatible counts and rejection rates rather than assuming posterior concentration validates the hidden cards.

## State consistency after an override

The rule proposal can update internal counters while deciding, before the selected move is executed. If that proposal is replaced, leaving those updates untouched would make the next decision depend on an action that never occurred. The override reconciles Lunatone's ability-used flag and adjusts the Premium Power Pro count by actual-play minus proposed-play. Rollout branches snapshot and restore the relevant policy state around their simulations. The matched plain/instrumented trace demonstrates consistency for that recorded replay; it is not a proof that every mutable field or exception path is correct.

## Representation and training scope

The submitted state token pools card embeddings for the Bench and hand rather than representing every visible entity as a separate attention token. Each legal option has a source/target-aware token; contextual scoring distinguishes card uses without a fixed-size action vocabulary. No option-position embeddings are added, but semantic encodings and stable index-based tie-breaking still matter. This supports an architectural description, not a measured end-to-end permutation-invariance claim.

The retained fine-tuning checkpoint uses a chronological game split of the 270-game corpus: 183 training games / 11,543 decisions, 33 validation games / 2,195 decisions, and 54 held-out games / 3,833 decisions. These total 17,571 decisions. Its five-corpus warm start includes this Lucario corpus and uses the same split fractions. The corpus count is not the training-only count. Checkpoint selection used validation: the training report shows that the validation-selected epoch 23 was retained although the held-out-selected epoch 19 scored 0.08 points higher on the held-out games. Neither these splits nor replay agreement certify best actions, opponent generalization or the absence of every possible cross-corpus duplicate. The legacy checkpoint lacks a feature-schema fingerprint, which reinforces the roadmap's separate feature-semantics repair requirement.

The state encoder takes the first twelve own-hand slots before pooling; its separate hand-count feature is capped at fifteen. Legal-option source/target features can still identify cards involved in choices. This is a bounded representation, not a complete arbitrary-size hand or perfect-information state. Padding masks exclude absent options from attention keys and set their final scores to negative infinity. The scores rank a menu; they are not calibrated probabilities of winning.

For multiple selections, the imitation runtime uses a training-fitted modal count for each selection context and clamps it to the permitted count and menu size. Its default count is at least one, even where selecting nothing is allowed. The submitted search only consumes qualifying single-choice alternatives; the later recurrent policy has a different multi-selection decoder. These details matter when proposing a replacement, not because a larger neural network automatically handles every action interface better.

## What sharing a sampled world guarantees

For each comparison, both proposed actions begin from the same constructed hidden state. Python-side world generation and branch policy randomness use deterministic seeds, and world construction restores the previous Python random state even on a filler error. The root seed hashes selected public fields: turn, action count, player slot, selection menu and logs. It is not a hash of every observation field or a unique state identifier.

Native-engine seeding has an additional local-evaluation hook. The confirmation harness uses a deterministic-engine bridge, and the selected instrumented trace explicitly enables deterministic search. The submitted entrypoint does not enable that local hook. Sharing sampled states therefore must not be presented as proof that every native random event is matched in deployed execution, or that local replay parity proves hosted-engine parity. Even identical starting seeds can lead to different later trajectories after different actions. A finalist port needs its own engine and end-to-end parity checks.

The strict twelve-world decision has a discrete score scale: with win/draw/loss values 1/0.5/0, the smallest positive mean difference is 1/24 when all twelve comparisons complete. That arithmetic describes the possible decision statistic, not a significance threshold, a minimum real-game benefit or an estimated error probability. We keep it out of the main report because the strict-mean rule already supplies the necessary reader understanding.

## Implementation checks and their limits

Local checks used the exact retained checkpoint with archived model code on synthetic encoded inputs. Permuting three option tokens permuted their scores within numerical tolerance; adding two masked options preserved the real-option scores and excluded padded choices. This tests the scoring layer for those fixtures, not end-to-end raw-observation permutation invariance, every menu, GPU parity or playing strength. A representation can behave consistently on synthetic tensors while still omitting strategically important information.

Separate archived-function fixtures checked deep-copy snapshots, in-place restoration of list/dict/set aliases, first-snapshot merge precedence, payoff perspective for both player slots, selection-count bounds and Python random-state restoration. Branch code checks rollout indices and treats unfinished branches as missing outcomes. These are useful mechanism checks, not proof that all native selection constraints, mutable fields and exception paths are covered. No additional game was played for this revision.
