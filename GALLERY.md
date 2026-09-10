# Gallery: research decisions and deck resources

These original diagrams complement the main report's inline decision trace. They distinguish deployed mechanisms, rejected alternatives and separate later learning. The wider canvas preserves all labels in Kaggle's 16:9 preview. The underlying diagram text and geometry are unchanged.

## Research questions and adoption decisions

![Three submitted stages above two rejected candidates and one separate local RL experiment. Broader search misses internal latency limits; richer imitation misses an accuracy subgroup limit. Future work tests resources and proposal sources before adoption.](research_decisions.png)

Our submitted Mega Lucario system generates a rule-based choice and a replay-trained alternative, compares eligible disagreements, and reconciles actual resource use after an override. The selected research examples show why broader search and richer imitation were rejected. The separate post-submission recurrent learner improved on replay imitation, not on the submitted hybrid. Future interventions remain proposed. The RL reward scale is loss/draw/win = -1/0/+1; its schedule was amended after development outcomes. These examples illustrate decisions, not unique algorithm families or a predicted finalist-hardware gain.

[Full-resolution image](research_decisions.png) | [Editable vector](research_decisions.svg).

## Mega Lucario resource interactions

![One shared Lunar Cycle per turn supports draw; discarded Energy can then prepare Benched attackers. Supporting tools provide card access, gusting, switching and conditional recovery; Wally does not clear Mega Brave's restriction.](deck_resources.png)

The submitted deck links hand development to attacker preparation: with Solrock present, Lunatone discards Basic Fighting Energy to draw, limited to one Lunar Cycle per turn across all copies. Aura Jab can reuse discarded Energy on the Bench. Damage modifiers and target access then support knockouts. Wally returns attached Energy only if it heals damage. The lower row organizes access, positioning and recovery tools; these are conditional interactions, not a guaranteed sequence. The exact counts are in the main report. Card roles and the one-opponent knockout-rule ablation do not establish optimal ratios.

[Full-resolution image](deck_resources.png) | [Editable vector](deck_resources.svg).

The diagrams contain no official card artwork or reconstructed gameplay footage. Hypothetical future work is labelled as proposed. See the [scope notice](NOTICE.md) and [version identity](VERSION.json).
