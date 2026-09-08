# Strategy writeup companion

Alessandro Flati and Roberto Mastropietro

An optional reference and evaluation guide for our [Kaggle Strategy submission](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle-challenge-strategy/writeups/new-writeup-1783092659035).

The submission link is access-dependent: our unauthenticated check returned 404. The submission was verified in our account; this repository does not change its visibility. The [competition page](https://www.kaggle.com/competitions/pokemon-tcg-ai-battle-challenge-strategy) provides the platform's current entry point.

Start with the writeup: it contains the complete competition submission. This companion helps readers locate original sources and interpret its existing results. It is not an additional scored report, an agent release, or a runnable reproduction package.

## Find the relevant material

| Reader question | Where to look |
|---|---|
| Which public baselines does the writeup acknowledge? | [Public baseline references](REFERENCES.md#public-baselines) |
| What is the background for attention and replay imitation? | [Method references](REFERENCES.md#method-background) |
| What does the reported paired improvement measure? | [Evaluation protocol and interpretation](EVALUATION.md) |
| Are distributed RL and regret learning part of the submitted agent? | [Further reading and implementation boundaries](REFERENCES.md#further-reading-not-the-submitted-algorithm) |
| Which version does this companion describe? | [Version identity](VERSION.json) |
| Where do data and source permissions come from? | [Scope and rights notice](NOTICE.md) |

The submitted system combines a rule baseline, a replay-trained alternative proposal, and bounded terminal comparisons. The reported complete-game comparison evaluates that combination, not the isolated contribution of each component. Later tests of the same submitted system do not retroactively become official competition results.

## Reading boundaries

- A reference establishes its own method or origin, not our performance.
- Public notebook scores and titles describe their authors' material, not a directly comparable benchmark for our entry.
- The companion does not contain competition data, card assets, replays, copied notebooks, model weights, agent code, or training code.
- The writeup remains understandable without opening this repository. No result depends on a reader executing external resources.
- Linked resources may require Kaggle sign-in or acceptance of the competition rules. They are not mirrored here.

This repository is maintained by the participants; it is not an official organizer resource or an endorsement by any referenced author.
