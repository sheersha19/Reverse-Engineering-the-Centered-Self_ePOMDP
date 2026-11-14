# Reverse-Engineering-the-Centered-Self_ePOMDP
Code for the MSc thesis Modelling Centering in Biological and Artificial Systems (Cognitive Science, IIT Delhi, 2026).
Author: Sheersha Roy, Vanshika Talus 
Supervisors: Prof. Sumeet Agarwal, Prof. Don W. F. Dcruz

Overview

Standard Computational models usually treat agency as reward maximisation, i.e. an agent is rational if it picks actions that maximise utility. Such models do not explain how an agent decides which problem it is solving, or how it works out where it stands within that problem. This project explores centering (Paul et al., 2025), the idea that problem-solving is perspectival: an agent acts effectively only after it locates itself within a problem representation. The philosophical basis is the work on self-locating beliefs and the essential indexical (Perry, 1979). A belief such as "I am standing here" motivates action in a way that "X is standing here" does not, because it is centred on the "I".

Centering is studied at two levels:
Meta-level (higher-level centering)-	Which problem representation should I centre on?	eg: Which of the four avatars is mine?
Lower level (self-location)-	Where am I within this problem, and what does that imply for how I act?	eg: I am in a dead-end corridor, so I should back out

The code builds POMDP (Partially Observable Markov Decision Process) agents in grid worlds, in three phases (Ongoing)

Phase 1: Standard POMDP for grid-world navigation

A simplified, single-avatar version of the grid navigation problem used in the avatar games. It serves as the reward-maximising baseline.

Environment: a grid with empty cells, hidden walls and one true goal. The agent sees only its own cell and its four adjacent cells.
Beliefs: the agent keeps a probability for each cell being empty, a wall or the goal, and a distribution over possible world states. It updates these with Bayes' rule after every observation.
Reward: the agent does not know where the goal is. After each move it receives R = C − ManhattanDistance(current cell, true goal), with C = 100. It uses this signal to eliminate candidate goal cells whose distance does not match, keeping a shrinking list of candidates.
Action selection: depth-limited lookahead that maximises expected value over valid actions.
Outcome: the agent reaches the goal in the minimum number of steps across the tested grids, including cases that force backtracking. However, the model has no representation of itself, so it does not capture centering.

Phase 2: Replication of the avatar games (mePOMDP)

A replication of Study 1 in Paul et al. (2025), whose meta-POMDP (mePOMDP) models centering at the meta-level. The aim was to test the original model's claim that it also represents self-location at a lower level.

Games: Logic, Contingency and Change Agent (switching). Each has four avatars (only one is the agent's own) and four candidate goals. The agent has a meta-level belief: over "theories", each theory being one avatar as self plus an action-to-movement mapping. The agent observes which avatar moves consistently with its actions and updates its beliefs by Bayesian inference. The agent explores until the ratio of its top two theories passes a confidence threshold. It then centres on that theory and walks to the nearest candidate goal using A*.

Agent types compared:
Optimal mePOMDP: attends to every movement of all four avatars.
Resource-limited mePOMDP: same inference, but with constraints on attention and memory, which makes it closer to human performance.

Phase 3: Enriched POMDP (ePOMDP)

Phase 3 combines the two earlier models. It keeps the mePOMDP's meta-level avatar identification and adds two enrichment layers to the lower-level problem-solving. Candidate goals are known to the agent in advance, and reward is given only on reaching the true goal.

Enrichment 1: Expected distance minimisation
Instead of heading for the nearest candidate (as A* does), the agent picks the candidate with the lowest expected distance to the true goal. This expectation is taken over its beliefs about candidate goals and world states, and includes the cost of backtracking if the candidate turns out to be wrong. The agent avoids being lured into a dead-end corridor that holds one nearby candidate, and visits clusters of candidates first. 

Enrichment 2: Corridor detection
The agent stores representations about itself in the environment: the entry point to a possible corridor, and a record of the last three steps with walls observed on both sides. It centres on one of two strategies:

P1: move to reduce the distance to the target goal.
P2: backtrack to the corridor's entry point.

When the stored representations indicate "I am in a corridor", the agent switches from P1 to P2, then recentres on P1 with an updated target.The agent forms self-locating beliefs ("I am in a dead-end corridor") and acts on them, leaving mid-way instead of walking to the end.

Repository structure
1. phase1_pomdp/          
2. phase2_mepomdp/       
3. phase3_epomdp/                   
4. README.md

Data and code sources
The avatar-game data and the code for the meta-POMDP and resource-limited meta-POMDP are from the repository of Paul et al. (2025): Reverse-engineering the centered self (https://github.com/ccolas/inferself/tree/main/src).The POMDP and ePOMDP code, and the comparison plots, were produced for this thesis.

References
Paul, L. A., Mills, T., Ullman, T., De Freitas, J., Colas, C., & Tenenbaum, J. (2025). Reverse-engineering the centered self.
Perry, J. (1979). The problem of the essential indexical. Noûs, 13(1), 3–21.
