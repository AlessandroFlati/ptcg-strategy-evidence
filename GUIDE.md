# Contribution and learning companion: reading guide

Alessandro Flati and Roberto Mastropietro

This guide supplements [the submitted English report](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle-challenge-strategy/writeups/new-writeup-1783092659035). [Version identity](VERSION.json) identifies the corresponding text; the [repository overview](README.md) is the shortest entry point.

| Reader question | Material |
|---|---|
| What engineering contribution does the report claim? | [Research questions and decisions](RESEARCH.md) |
| How does the contribution differ from public RL and gated-learning reports? | [Public context and the specific integration claim](RESEARCH.md#public-context-and-the-specific-integration-claim) |
| What does the new recurrent learning result actually compare? | [Separate local reinforcement-learning (RL) evaluation](LEARNING.md) |
| How do the submitted and later networks differ? | [Architecture and comparator map](LEARNING.md#architecture-and-comparator-map) |
| What does V-trace correct, and what does the decoder select? | [Probability, return and history contracts](LEARNING.md#what-the-correction-and-decoder-mean) |
| How do paired outcomes, weighting and missing coverage affect the interpretation? | [Outcome tables and sensitivity explanations](LEARNING.md#reading-paired-results-without-conflating-the-experiments) |
| What does one confirmation pair share, and what can still differ? | [Pairing under the deterministic bridge](LEARNING.md#why-the-paired-records-matter) |
| Is the knockout-rule ablation's "corrected" baseline the submitted parent? | [Comparator identity for the card-policy evidence](RESEARCH.md#what-the-evidence-supports) |
| Why is the official rank reported without a variance estimate? | [Official rank, verification and variance](GUIDE.md#official-rank-verification-and-variance) |
| Which game terms does the report use without defining them? | [Glossary of game terms](GUIDE.md#glossary-of-game-terms-used-in-the-report) |
| Where is each companion section? | [Section map](GUIDE.md#section-map-of-the-companion-documents) |
| Which machine-learning and evaluation terms does the report use without defining them? | [Glossary of method terms](GUIDE.md#glossary-of-method-terms-used-in-the-report) |
| Where should a particular kind of reader start? | [Reading paths by perspective](GUIDE.md#reading-paths-by-perspective) |
| Which technical terms appear only in the companion? | [Terms used only in the companion](GUIDE.md#terms-used-only-in-the-companion) |
| Exactly when can the submitted rule decision be replaced? | [Submitted decision boundaries](RESEARCH.md#exact-boundaries-of-the-submitted-decision-rule) |
| What is shared between the two simulated alternatives? | [Sampled worlds and local seeding](RESEARCH.md#what-sharing-a-sampled-world-guarantees) |
| What could additional compute enable, and what would disprove its value? | [Prospective resource programme](ROADMAP.md) |
| Why might a better standalone learner fail to improve this hybrid? | [Proposal experiment diagnostics](ROADMAP.md#diagnose-the-proposal-experiment-before-scaling-it) |
| How is the submitted hybrid's weighted improvement calculated? | [Existing published evaluation guide](https://github.com/AlessandroFlati/ptcg-strategy-evidence/blob/main/EVALUATION.md) |
| Where are the public baseline and method references? | [Existing reference guide](https://github.com/AlessandroFlati/ptcg-strategy-evidence/blob/main/REFERENCES.md) |

For a technical reading, start with the submitted system in Research, then inspect Learning's separate architecture and comparator before reading its results. Roadmap specifies the controlled experiment that would connect the two. The later learner's recurrent own-action buffer does not implement opponent-history or best-of-three (BO3) adaptation, and its absolute win rates cannot be ranked against the submitted hybrid's differently weighted evaluation, which also compared different arms.

The English report remains the primary argument; external material is optional. The [Japanese translation](WRITEUP_JA.md) translates the same report without adding scientific claims. It was AI-produced and checked for numerical and semantic alignment; native-language review has not been performed.

This edition supplies explanations and aggregate arithmetic, not a runnable agent, raw-data release, full independent gameplay reproduction or a new official score. It contains no card artwork, replays, credentials, model weights or competition binaries. Pokemon names identify the relevant game objects; their rights are not claimed or open-sourced by this companion. Existing source-specific permissions and attribution remain applicable.

The [version record](VERSION.json) binds these documents to the English source. The [gallery guide](GALLERY.md) opens with an introduction to the argument, followed by two detailed diagrams with explanatory captions. Internal audit ledgers and raw research records are not included in this release.

The tables and diagnostics reuse retained records. The historical evaluation guide already documents the hybrid's weighted and pooled summaries; they are not new discoveries or new games in this edition. The later learner's contingency table, fixed-opponent uncertainty check and implementation details are provided in [Learning](LEARNING.md).

For the deck question, start with [access, timing and competing resources](RESEARCH.md#deck-strategy-access-timing-and-competing-resources). Its search-card table explains why the deck's twelve search Items are not interchangeable, while the text separates card mechanics, implemented priorities and measured outcomes.

## What can be checked from this companion

| Reader check | Available material | Boundary |
|---|---|---|
| Understand the submitted system and its relation to later learning | Main report, Research, and the architecture/comparator map | An explanation is not a runnable implementation |
| Recalculate the hybrid's weighted gain and reported uncertainty | Opponent aggregates and formulas in the existing public evaluation guide | Participant-authored summaries cannot authenticate the underlying games |
| Recalculate later RL win/draw/loss totals and mean reward difference | Marginal and paired-outcome tables in the Learning page | The compact tables do not contain every seed or per-game outcome |
| Check public method and baseline attribution | Original-resource links in the published reference guide | Mutable notebook versions are not necessarily the locally adapted source |
| Distinguish exact retained artifacts | Published version record and the checkpoint/final-result hashes reported here | A hash is an identity commitment; unavailable bytes cannot be independently checked from the hash alone |
| Repeat the original gameplay or train from raw data | Requires permitted simulator, exact policies, weights, records and environment | These are not distributed in this companion; do not promise public end-to-end reproduction |
| Assess prospective hardware or BO3 value | Explicit comparison arms and acceptance contracts in Roadmap | Designs are not measured gains or evidence of resource access |

For a quick review, use the opening gallery overview to orient yourself, then read the main argument and its three tables. Use the research-decision diagram to inspect the selection logic and the deck-resource diagram for card interactions. The architecture map answers a deeper model question without adding another competing figure. Follow the arithmetic only when checking a particular result. This reading order keeps optional evidence from becoming a prerequisite for understanding the submission.

The shortest evidence-to-next-test route is: the main report's complete-game confirmation supports the submitted combination locally; Learning's separate result supports its recurrent learner over that learner's own parent; Roadmap tests whether replacing only the submitted proposal source connects those findings. The missing connection is a controlled integration result, not another way of restating the two existing gains.

## Official rank, verification and variance

The report states the official result as rank 282 at 938.7 without a variance estimate. Organizers wrote on 2 September that a Strategy writeup may note perceived randomness in the final Simulation ranking, that the final leaderboard score is an input to the Strategy score, and that Bradley-Terry re-evaluation is not applied to this competition; on 8 September they stated that the competition results had been verified. We keep the rank as stated because no retained analysis of the final evaluation's sampling exists for our own entry, and an estimate built from other participants' public simulations would not be our evidence. Our uncertainty statements therefore stay with the local paired protocol, where the records exist. The verified rank remains the performance input; the local results neither replace nor re-weight it.

The team's two final Simulation entries, submitted on 16 August at 17:42 and 21:37 UTC, were byte-identical copies of one archive; the leaderboard row counts two submissions and one team score. The official result therefore belongs to that single program, and the report describes it once rather than as two agents or a selected copy.

## Glossary of game terms used in the report

The table explains game terms used in the report. Its summaries follow the official competition card data and supplied simulator; simulator identifiers locate the relevant rules. Definitions are paraphrases and retain effect-specific exceptions.

| Term | Meaning in the simulator | Source |
|---|---|---|
| Active Spot and Bench | Each player keeps one Active Pokemon and, by default, up to five Benched Pokemon. The Active Pokemon declares attacks; attack targets and effects can also include Benched Pokemon. A player with no Pokemon in either place loses. | Simulator areas and finish check; Hippowdon's Super Sandstorm (card 23) damages eligible Benched Pokemon |
| Prize cards | Six cards set aside face down at setup. Knocking Out an opposing Pokemon takes one Prize, two for a Pokemon ex and three for a Mega Pokemon ex; taking the last Prize wins. Mega Lucario ex therefore gives up three Prizes and Hariyama one. | `PRIZE_SIZE = 6`; prize-count rule; finish check (`Prize0`) |
| Knocked Out | A Pokemon whose accumulated damage reaches its HP is Knocked Out, and its opponent takes the Prizes above. The report's "knockout threshold" is the damage that reaches the target's remaining HP. | Damage application sets the knockout flag when damage reaches maximum HP |
| Weakness and Resistance | Attack damage to the Active Pokemon normally applies matching Weakness and Resistance: double damage for Weakness, subtract 30 for Resistance, with damage never below zero. Bench damage normally omits them; placing damage counters is a different effect. Mega Lucario ex has Psychic Weakness and no Resistance. | Damage calculation; cards 23 and 678; effect-specific rules remain applicable |
| Energy | Cards attached to a Pokemon to pay attack and retreat costs. The deck's only Energy is Basic Fighting Energy: Aura Jab costs one, Mega Brave two. Premium Power Pro is an Item, not Energy; it adds 30 damage before Weakness and Resistance for the turn. | Cards 6, 678 and 1141 |
| Basic and Stage 1 | A Basic Pokemon is played from the hand directly; a Stage 1 Pokemon is played onto its named previous stage. Hariyama evolves from Makuhita and Mega Lucario ex from Riolu, so both need their Basic in play first. | Card data stage columns; simulator `EvolutionType` |
| Rule Box | Pokemon ex, including Mega Pokemon ex, carry a Rule Box. Poke Pad can fetch only a Pokemon without one, so it never finds Mega Lucario ex, while Ultra Ball can. | Poke Pad text (card 1152); card 678 rule "Mega Pokemon ex" |
| Gust | Informal name for moving an opposing Benched Pokemon into the Active Spot, which is what Boss's Orders and Hariyama's Heave-Ho Catcher Ability do. | Cards 1182 and 674 |
| Retreat | Moving the Active Pokemon to the Bench by paying its retreat cost in attached Energy; Switch does the same without paying. Mega Lucario ex's retreat cost is 2. Leaving the Active Spot clears the Mega Brave restriction. | Card retreat column; Switch text (card 1123); simulator retreat cost and move logic |
| Deck-out | A player whose deck is empty when their turn starts loses before drawing. Lunatone draws from the deck. Ultra Ball's two-card cost discards from the hand; its separate search can remove one Pokemon from the deck. The rule policy limits Lunatone draw at ten or fewer cards. | Turn start (`Deck0`); Ultra Ball (card 1121), `costHandTrash(2)` and deck-to-hand search |
| Trainers: Item, Supporter and Tool | Trainer cards played from the hand. In this deck Ultra Ball, Switch, Premium Power Pro, Fighting Gong and Poke Pad are Items; Boss's Orders, Judge, Lillie's Determination and Wally's Compassion are Supporters; Hero's Cape is an ACE SPEC Pokemon Tool. | Card data category column; simulator `CardType` |

Definitions describe the simulator's rules, not a claim about optimal play. The main report keeps its own wording; this table is companion material, not part of the submitted word count.

## Section map of the companion documents

Every section of the three companion documents in reading order, so that a reviewer can reach any part directly. Anchors are the rendered heading identifiers.

| Document | Sections |
|---|---|
| [Research](RESEARCH.md) (11 sections) | [Our contribution](RESEARCH.md#our-contribution); [Deck strategy: access, timing and competing resources](RESEARCH.md#deck-strategy-access-timing-and-competing-resources); [Hypothesis and decision evidence](RESEARCH.md#hypothesis-and-decision-evidence); [Research breadth without conflating methods](RESEARCH.md#research-breadth-without-conflating-methods); [Worked decision provenance](RESEARCH.md#worked-decision-provenance); [Exact boundaries of the submitted decision rule](RESEARCH.md#exact-boundaries-of-the-submitted-decision-rule); [Opponent belief and hidden-state approximation](RESEARCH.md#opponent-belief-and-hidden-state-approximation); [State consistency after an override](RESEARCH.md#state-consistency-after-an-override); [Representation and training scope](RESEARCH.md#representation-and-training-scope); [What sharing a sampled world guarantees](RESEARCH.md#what-sharing-a-sampled-world-guarantees); [Implementation checks and their limits](RESEARCH.md#implementation-checks-and-their-limits) |
| [Learning](LEARNING.md) (9 sections) | [Exact comparison](LEARNING.md#exact-comparison); [What the recurrent learner actually implements](LEARNING.md#what-the-recurrent-learner-actually-implements); [What the correction and decoder mean](LEARNING.md#what-the-correction-and-decoder-mean); [Locked local final evaluation](LEARNING.md#locked-local-final-evaluation); [Selection, amendments and identity](LEARNING.md#selection-amendments-and-identity); [Reading paired results without conflating the experiments](LEARNING.md#reading-paired-results-without-conflating-the-experiments); [Missing coverage, safety and objective boundaries](LEARNING.md#missing-coverage-safety-and-objective-boundaries); [Why the paired records matter](LEARNING.md#why-the-paired-records-matter); [Cross-deck context](LEARNING.md#cross-deck-context) |
| [Roadmap](ROADMAP.md) (9 sections) | [Ordered experiment contracts](ROADMAP.md#ordered-experiment-contracts); [A resource budget that survives a complete game](ROADMAP.md#a-resource-budget-that-survives-a-complete-game); [Make the next result answer one question](ROADMAP.md#make-the-next-result-answer-one-question); [Experiments that isolate the proposed improvement](ROADMAP.md#experiments-that-isolate-the-proposed-improvement); [Diagnose the proposal experiment before scaling it](ROADMAP.md#diagnose-the-proposal-experiment-before-scaling-it); [Preserve the measurement when implementation changes](ROADMAP.md#preserve-the-measurement-when-implementation-changes); [Why BO3 needs match-level evidence](ROADMAP.md#why-bo3-needs-match-level-evidence); [Test deck changes together with their policy consequences](ROADMAP.md#test-deck-changes-together-with-their-policy-consequences); [References and limits](ROADMAP.md#references-and-limits) |

## Glossary of method terms used in the report

The report also uses machine-learning and evaluation terms without defining them. Each row states what the term means in this work, as implemented in the archived submission, the retained learning code or the retained verifier, and where the companion explains it further. Primary references are listed in the [published reference guide](https://github.com/AlessandroFlati/ptcg-strategy-evidence/blob/main/REFERENCES.md).

| Term | Meaning in this work | Where explained; source |
|---|---|---|
| Replay imitation (behavioral cloning) | The network is trained to reproduce the option a recorded player chose: a cross-entropy loss over the legal menu with the chosen option as the target. | [Representation and training scope](RESEARCH.md#representation-and-training-scope); archived trainer's listwise loss |
| Warm start and fine-tuning | Training first on five public-replay corpora, then continuing from that checkpoint on the 270-game Mega Lucario corpus; the shipped checkpoint records its initial checkpoint and 30 further epochs. | [Representation and training scope](RESEARCH.md#representation-and-training-scope); shipped checkpoint arguments |
| Chronological split and checkpoint selection | Games are ordered by time; the last 20% are held out, and the last 15% of the remaining training games form a validation slice. The epoch with the best validation accuracy is retained, so the held-out accuracy is not used for selection. | [Representation and training scope](RESEARCH.md#representation-and-training-scope); archived trainer's split and epoch selection |
| Self-attention | In each of the 8 layers, every token (one state token and one token per legal option) is compared with every other token, so an option's score depends on the state and on the competing options. | [Architecture and comparator map](LEARNING.md#architecture-and-comparator-map); Vaswani et al. (2017) in the reference guide |
| Opponent belief and posterior mass | A probability distribution over ten reference decks, updated from revealed cards; search opens only when at least 0.95 of that probability sits on one reference. | [Opponent belief and hidden-state approximation](RESEARCH.md#opponent-belief-and-hidden-state-approximation); archived peak gate 0.95 |
| Hidden-state completion and filler cards | Visible cards are subtracted from the reference list and the remainder is shuffled into the hidden slots; when the list runs short, filler cards complete the deck. Twelve completions are drawn per comparison. | [What sharing a sampled world guarantees](RESEARCH.md#what-sharing-a-sampled-world-guarantees); archived determinizer, twelve worlds |
| Terminal rollout | From each completed hidden state, play continues to the end of the game with the baseline and reference-opponent policies; the game scores 1 for a win, 0.5 for a draw and 0 for a loss. | [Exact boundaries of the submitted decision rule](RESEARCH.md#exact-boundaries-of-the-submitted-decision-rule); archived outcome function |
| Paired standard error | Each pair contributes one outcome difference; the standard error is the standard deviation of those differences divided by the square root of the number of pairs, then combined across opponents with squared weights. | [Reading paired results without conflating the experiments](LEARNING.md#reading-paired-results-without-conflating-the-experiments); retained verifier |
| Normal-approximation 90% interval | The weighted gain plus or minus 1.645 standard errors; it assumes the gain is approximately normally distributed. | [Reading paired results without conflating the experiments](LEARNING.md#reading-paired-results-without-conflating-the-experiments); retained verifier |
| Actor-critic | One network with two outputs: the actor scores options and the critic predicts the eventual game reward. The critic serves as a training target and baseline, not as the submitted search's payoff. | [Recurrent mechanism](LEARNING.md#recurrent-mechanism); retained learning code |
| V-trace and clipped corrections | Each recorded action's probability under the current weights is divided by its probability under the rollout policy; the ratios are capped (all caps 1.0 in the retained protocol) before they weight the value targets and policy advantages. | [What the correction and decoder mean](LEARNING.md#what-the-correction-and-decoder-mean); retained learning code and protocol |
| Gated recurrent unit (GRU) | A small recurrent network that summarizes the latest sixteen own selected-action tokens into one vector for the actor and critic. | [Recurrent mechanism](LEARNING.md#recurrent-mechanism); retained model |
| Terminal-only reward | Completed games provide environmental reward -1/0/+1 for loss/draw/win, without intermediate damage or Prize rewards. Entropy and imitation-reference terms also affect the training loss; incomplete training games use a separate -0.25 convention. | [Reward and failure boundaries](LEARNING.md#missing-coverage-safety-and-objective-boundaries); retained learner and rollout |
| Bootstrap 95% interval | The 5,000 paired differences are resampled with replacement 10,000 times (seed 72999); the 2.5th and 97.5th percentiles of the resampled means give the interval +0.2384 to +0.2992, which this package's evidence audit recomputes. | [Locked local final evaluation](LEARNING.md#locked-local-final-evaluation); retained protocol and final record |
| Learned value estimates and offline Q | Networks trained to predict a game's outcome from logged states and actions. The retained finding is that outcome prediction did not rank the untaken sibling actions reliably. | [Research breadth without conflating methods](RESEARCH.md#research-breadth-without-conflating-methods); retained synthesis record |
| Policy selector | A model that would pick, from public information, which of several policies to follow in a state. The hindsight best policy showed headroom, but no deployable selector reached it. | [Research breadth without conflating methods](RESEARCH.md#research-breadth-without-conflating-methods); retained synthesis record |
| Regret learning (Deep CFR branch) | Our experiment estimates regrets over a restricted action set and shifts probability toward positive estimates. It branches over RETREAT, ATTACK and END (types 12, 13, 14), plus one representative for other options. This limited implementation is not a full-game Deep CFR result or a guaranteed policy improvement. | [Research breadth without conflating methods](RESEARCH.md#research-breadth-without-conflating-methods); retained branch code |

These meanings are specific to this work. They are not general definitions, and none of them asserts that a method worked beyond the reported evidence.

## Reading paths by perspective

The companion serves several kinds of reader. Each path lists companion sections in a suggested order; the main report remains the argument itself.

| Reader | Suggested order |
|---|---|
| New to the card game | [Glossary of game terms](GUIDE.md#glossary-of-game-terms-used-in-the-report); [Deck strategy: access, timing and competing resources](RESEARCH.md#deck-strategy-access-timing-and-competing-resources); [Worked decision provenance](RESEARCH.md#worked-decision-provenance); then the main report's deck section and Figure 1 |
| Machine-learning reviewer | [Architecture and comparator map](LEARNING.md#architecture-and-comparator-map); [Glossary of method terms](GUIDE.md#glossary-of-method-terms-used-in-the-report); [Representation and training scope](RESEARCH.md#representation-and-training-scope); [What the correction and decoder mean](LEARNING.md#what-the-correction-and-decoder-mean); [Experiments that isolate the proposed improvement](ROADMAP.md#experiments-that-isolate-the-proposed-improvement) |
| Evaluation specialist | [Why the paired records matter](LEARNING.md#why-the-paired-records-matter); [Reading paired results without conflating the experiments](LEARNING.md#reading-paired-results-without-conflating-the-experiments); [What the evidence supports](RESEARCH.md#what-the-evidence-supports); [Make the next result answer one question](ROADMAP.md#make-the-next-result-answer-one-question) |
| Skeptical judge | [Hypothesis and decision evidence](RESEARCH.md#hypothesis-and-decision-evidence); [Selection, amendments and identity](LEARNING.md#selection-amendments-and-identity); [Missing coverage, safety and objective boundaries](LEARNING.md#missing-coverage-safety-and-objective-boundaries); [Official rank, verification and variance](GUIDE.md#official-rank-verification-and-variance) |

These perspectives are reading aids chosen by the authors, not descriptions of actual judges or their preferences.

## Terms used only in the companion

The companion documents use further technical terms that the main report avoids. Each row states the meaning in this work and its source.

| Term | Meaning in this work | Source |
|---|---|---|
| Token and embedding | A token is one vector that the attention layers process; an embedding is the learned vector for a card, attack, option type or area identifier from which tokens are built. | Archived trainer's embedding tables |
| Pooled | Averaging the embeddings of a variable number of cards (Bench, hand) into one vector, ignoring empty slots, so that the state token has a fixed size. | Archived trainer's mean pooling |
| Mask | Marks absent options so that attention ignores them and their final scores are set to negative infinity, which makes them impossible to select. | Archived trainer's padding mask |
| Logit | A raw score before it is turned into a probability; softmax turns the legal options' logits into probabilities that sum to one. | Archived trainer's log-softmax loss |
| Entropy penalty | A training term with coefficient 0.01 that discourages the later learner's policy from concentrating all probability on one option. | Retained protocol and learner |
| Advantage | The corrected return advantage used to weight the actor update. With other terms held aside, a positive advantage encourages the recorded action; regularization and shared parameters also affect the update. | Retained V-trace learner's policy advantages |
| Discount | The factor 0.997 by which later reward is scaled per decision when value targets are formed. | Retained protocol |
| Residual scores | History-conditioned additions to the trainable imitation-initialized encoder's option scores. The later actor uses base scores plus this residual; a separate frozen parent supplies the regularization reference. Evaluation mode does not freeze gradients. | Retained model's `base` and `parent_base`; optimizer includes parameters with gradients |
| Canonical sequential decoder | For decisions that select several options, the later policy orders the options canonically and chooses one position at a time, honouring the minimum and maximum counts with a stop action. | Retained model |
| Off-policy and on-policy | Learning from trajectories generated by an older or different policy, which needs correction (off-policy), versus from the current policy (on-policy). | Retained learner's behavior probabilities |
| Dropout | Randomly zeroing activations during training (rate 0.15 in the shipped checkpoint); evaluation mode disables it. | Shipped checkpoint arguments |
| Estimand and stratified standard error | The estimand is the quantity a comparison aims to estimate. Fixed-opponent uncertainty combines paired variances using the specified weights: historical weights for the hybrid's primary summary, equal opponent weights for the later RL summary. | Learning's paired-results and weighting sections |
| Sibling action | Another legal action available in the same decision; the rejected value branches failed to rank siblings. | Retained synthesis record |
| Hindsight oracle | A selector that sees the tested outcomes before choosing among the tested policies. Its apparent headroom applies to those policies and records; it is not a universal bound or a deployable public-information selector. | Research breadth section |
| Prior and posterior | The reference-deck probabilities before revealed cards are counted (the prior, hard-coded in the archived variant) and after they are counted (the posterior). | Archived reference specifications |
| Bradley-Terry | A paired-comparison rating model; the organizers stated that future simulations will use it to re-evaluate the leaderboard and that it is not applied to this competition. | [Organizers' recap and replies](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle/discussion/738791) |

As with the other glossaries, these meanings are specific to this work and assert nothing beyond the reported evidence.
