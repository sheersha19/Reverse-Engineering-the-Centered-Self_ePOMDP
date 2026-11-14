# Reverse-Engineering-the-Centered-Self_ePOMDP
Code for the MSc thesis Modelling Centering in Biological and Artificial Systems (Cognitive Science, IIT Delhi, 2026).
Author: Sheersha Roy, Vanshika Talus 
Supervisors: Prof. Sumeet Agarwal, Prof. Don W. F. Dcruz

Standard Computational models usually treat agency as reward maximisation, i.e. an agent is rational if it picks actions that maximise utility. Such models do not explain how an agent decides which problem it is solving, or how it works out where it stands within that problem. This project explores centering (Paul et al., 2025), the idea that problem-solving is perspectival: an agent acts effectively only after it locates itself within a problem representation. The philosophical basis is the work on self-locating beliefs and the essential indexical (Perry, 1979). A belief such as "I am standing here" motivates action in a way that "X is standing here" does not, because it is centred on the "I".Centering is studied at two levels of Meta-level (higher-level centering)-	Which problem representation should I centre on?	eg: Which of the four avatars is mine? and Lower level (self-location)-	Where am I within this problem, and what does that imply for how I act?	eg: I am in a dead-end corridor, so I should back out. The code builds POMDP (Partially Observable Markov Decision Process) agents in grid worlds, in three phases. 

Phase 1: Standard POMDP for grid-world navigation (single-avatar version of the grid navigation problem with reward maximisation as goal)

The grid has empty cells, hidden walls and one true goal. The agent sees only its own cell and its four adjacent cells, keeps a probability for each cell being empty, a wall or the goal, and a distribution over possible world states. It updates these with Bayes' rule after every observation. After each move it receives an unknown reward (calculated using Manhattan Distance)and uses this signal to eliminate candidate goal cells whose distance does not match, keeping a shrinking list of candidates.The agent selects actions based on depth-limited lookahead that maximises expected value over valid actions.

Phase 2: Replication of the avatar games (mePOMDP)

Performance is measured across agent types, namely the Optimal mePOMDP and Resource-limited mePOMDP and game types (Logic, Contingency and Switching Embodiments) from the original model. 

Phase 3: Adding Enrichment layers to the standard POMDP in Phase 1

The agent chooses its target by weighing which candidate goal is cheapest to reach overall, instead of heading for the nearest one, which avoids costly detours. It also keeps simple representations of where it is, and uses them to switch strategy, such as backing out when its position suggests a dead end.

Repository structure
1. phase1_pomdp                
2. phase3_epomdp (Phase 2 in the inferself repository linked at the end)                   
3. README.md


Data and code sources
The avatar-game data and the code for the meta-POMDP and resource-limited meta-POMDP are from the repository of Paul et al. (2025): Reverse-engineering the centered self (https://github.com/ccolas/inferself/tree/main/src).The POMDP and ePOMDP code, and the comparison plots, were produced for this thesis.

References
Paul, L. A., Mills, T., Ullman, T., De Freitas, J., Colas, C., & Tenenbaum, J. (2025). Reverse-engineering the centered self.

Perry, J. (1979). The problem of the essential indexical. Noûs, 13(1), 3–21.
