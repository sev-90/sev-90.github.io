---
layout: archive
title: "Papers"
permalink: /publications/
author_profile: true
---

2025
----
7. [**Dynamic penalty-based dispatching decision-making for improved EMS response in urban environments: a heuristic approach**](https://www.frontiersin.org/journals/future-transportation/articles/10.3389/ffutr.2025.1540502/abstract)\
Sevin Mohammadi, Audrey Olivier, Andrew W. Smyth, Frontiers in Future Transportation.

2024
----
6. [**NLP-enabled Trajectory Map-matching in Urban Road Networks using a Transformer-based Encoder-decoder**](https://doi.org/10.48550/arXiv.2404.12460)\
Sevin Mohammadi, Andrew W. Smyth, under review.

2023
----
5. [**Probabilistic prediction of trip travel time and its variability using hierarchical Bayesian learning**](https://doi.org/10.1061/AJRUA6.RUENG-981)\
Sevin Mohammadi, Audrey Olivier, Andrew W. Smyth, ASCE-ASME Journal of Risk and Uncertainty in Engineering Systems, Part A. [Code](https://github.com/sev-90/HierarchicalBayesianRegression)
4. [**Bayesian neural networks with physics-aware regularization for probabilistic travel time modeling**](https://doi.org/10.1111/mice.13047)\
Audrey Olivier, Sevin Mohammadi, Andrew W. Smyth, Matt Adams, Computer‐Aided Civil and Infrastructure Engineering.


2022
----
3. [**Data analytics for improved closest hospital suggestion for EMS operations in New York City**](https://www.sciencedirect.com/science/article/pii/S2210670722004188)\
Audrey Olivier, Matt Adams, Sevin Mohammadi, Andrew Smyth, Kathleen Thomson, Timothy Kepler, Monish Dadlani, Sustainable Cities and Society, 2022.


2021
----
2. [**Simulating NYC Hospital Load Balancing During COVID-19,**](https://doi.org/10.1109/WSC52266.2021.9715419)\
   E. L. de Larrea, H. Lam, E. Sanabria, J. Sethuraman, S. Mohammadi, A. Olivier, A. Smyth et al., IEEE: Winter Simulation Conference, 2021
1. [**The Role of Drivers' Social Interactions in their Driving Behavior: Empirical Evidence and Implication for Traffic Flow,**](https://doi.org/10.1016/j.trf.2021.04.002)\
   Sevin Mohammadi, Ramin Arvin, Asad J Khattak, Subhadeep Chakraborty, Transportation Research Part F: Traffic Psychology & Behavior, 2021.


{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
