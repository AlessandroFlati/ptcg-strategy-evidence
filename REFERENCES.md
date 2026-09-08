# Annotated references

These are selected original resources relevant to the submitted writeup, not an exhaustive bibliography or a claim of priority. Links identify the original publishers; no linked content is bundled here.

## Competition context

- [Strategy overview and evaluation](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle-challenge-strategy): authoritative task description, submission requirements and judging criteria.
- [Strategy rules](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle-challenge-strategy/rules): controlling participation, data-use and sharing terms. This guide does not replace them.
- [Simulation competition](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle): original environment and official performance context. Obtain restricted materials from the organizer's permitted channel, not from this repository.

## Public baselines

The writeup credits public-policy foundations by Kiyota, Kojimar, Masamikobayashi and Biohack44. These links locate relevant original notebooks. They are attribution pointers, not a claim that their current versions are byte-identical to our locally adapted policies.

- Kiyota, [A Sample Rule-Based Agent Mega Lucario ex Deck](https://www.kaggle.com/code/kiyotah/a-sample-rule-based-agent-mega-lucario-ex-deck): official sample-agent background for the deck-specific rule-policy lineage.
- Kojimar, [Simple Baseline + Matchup Tests](https://www.kaggle.com/code/kojimar/simple-baseline-matchup-tests): public baseline identified in the provenance of our adapted rule policies.
- Masamikobayashi, [Archaludon sample agent](https://www.kaggle.com/code/masamikobayashi/a-sample-archaludon-75-wr-vs-my-1300-starmie): source pointer for one locally adapted opponent-policy family. Numbers in the original notebook title are its author's claims, not our evaluation.
- Masamikobayashi, [Cynthia Garchomp sample agent](https://www.kaggle.com/code/masamikobayashi/a-sample-cynthia-garchomp-ex-deck): source pointer for another opponent-policy family.
- Biohack44, [Public Crustle baseline](https://www.kaggle.com/code/biohack44/beating-the-day-2-new): source pointer for an opponent-policy family used in our local research.

Notebook pages are mutable. Consult their version histories for the public resource's history; the submitted-system archive identity in [VERSION.json](VERSION.json) identifies our retained artifact, not a downloadable release of those notebooks. Attribution does not authorize redistribution or imply endorsement.

## Method background

- Vaswani et al. (2017), [Attention Is All You Need](https://arxiv.org/abs/1706.03762): background for the self-attention mechanism used to contextualize state and option tokens. Our option scorer is a task-specific application, not a reproduction of the paper's translation system or results.
- Ross and Bagnell (2010), [Efficient Reductions for Imitation Learning](https://proceedings.mlr.press/v9/ross10a.html): background for distribution shift when a learned policy changes the states it visits. This helps explain why held-out action agreement is not sufficient evidence of complete-game strength. We do not claim to implement the paper's proposed algorithms.

## Further reading, not the submitted algorithm

- Espeholt et al. (2018), [IMPALA: Scalable Distributed Deep-RL with Importance Weighted Actor-Learner Architectures](https://proceedings.mlr.press/v80/espeholt18a.html): context for distributed actor-learner training and V-trace correction. It does not establish a hardware speedup or RL result for the agent described in our submission.
- Brown et al. (2019), [Deep Counterfactual Regret Minimization](https://proceedings.mlr.press/v97/brown19b.html): context for neural regret learning in imperfect-information games. The submitted terminal-search policy is not Deep CFR; this paper supplies neither a convergence guarantee nor measured strength for our system.

The last two entries are optional background for readers considering extensions. They are not additional claimed contributions or substitutes for implementation and evaluation.
