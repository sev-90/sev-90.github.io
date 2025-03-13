---
title: "EMS simulation"
excerpt: "For this project, I edveloped a Discrete Event Simulation (DES) to simulate emergency response in an end-to-end manner. "
collection: portfolio
---

Developed a Discrete Event Simulation (DES) in Python to model emergency response end-to-end. 

Built using SimPy for simulation, Pygame for visualization, and NetworkX for network modeling.

Abstract
The dispatching process for emergency medical services (EMS) is inherently sequential and dynamic, with each decision impacting future resource availability. Conventional greedy approaches that dispatch the nearest available unit without accounting for the supply-demand dynamics within its catchment area can lead to suboptimal outcomes. In this project, I introduce a dynamic penalty-based dispatching strategy that penalizes dispatches from high-demand, low-coverage areas for low-priority calls, with the aim of conserving resources for potential high-priority emergencies. First, a discrete event simulation (DES) model is developed to replicate EMS operations under various dispatching policies in synthetic environments. The proposed heuristic dispatching policy is tested in large-scale simulations, evaluated through multiple randomized episodes, and compared against the greedy policy. 

# Discrete event simulation
In order to validate the effectiveness of the proposed penalty metric in guiding the policy toward the optimal policy, I build an end-to-end discrete event simulation model (DES) that synthetically represents the operation of EMS response system. DES allows decision-makers to experiment with changes in system configurations, resource allocations, or operational policies to observe potential impacts on system performance without disrupting the real system. The typical workflow of a DES model starts with the initialization of the system state, followed by the sequential processing of events, and the consequent updating of the system state after each event. The simulation runs until a specified end condition is met, such as reaching a predefined simulation time or the completion of a set number of events. 
In the discrete event simulation model, an incident generator simulates the generation of emergency calls across various locations using a Poisson distribution, with the rate of incidents varying spatially. Such spatial variation can arise from factors like non-uniform population densities, demographic distributions, or or disparities in urban infrastructure across different areas. 

<p align="center">
<img src="/images/DES_time.png" width="600" height="600" />
</p>

<p align="center">
<img src="/images/DES_loc.png" width="400" height="400" />
</p>

<p align="center">
<img src="/images/ems.gif" width="400" height="400" />
</p>
