---
layout: archive
title: "Papers"
permalink: /publications/
author_profile: true
---
2024
----
1. [**Surrogate Modeling of Trajectory Map-matching in Urban Road Networks using Transformer Sequence-to-Sequence Model**](https://doi.org/10.48550/arXiv.2404.12460)\
Sevin Mohammadi, Andrew W. Smyth, Revision submitted to  IEEE Intelligent Transportation Systems.
2. **Dynamic penalty-based dispatching decision-making for improved EMS response in urban environments: a heuristic approach**\
Sevin Mohammadi, Audrey Olivier, Andrew W. Smyth, Submitted to Sustainable Cities and Socity.

2023
----
1. [**Probabilistic prediction of trip travel time and its variability using hierarchical Bayesian learning**](https://doi.org/10.1061/AJRUA6.RUENG-981)\
Sevin Mohammadi, Audrey Olivier, Andrew W. Smyth, Accepted in ASCE-ASME Journal of Risk and Uncertainty in Engineering Systems, Part A.
2. [**Bayesian neural networks with physics-aware regularization for probabilistic travel time modeling**](https://doi.org/10.1111/mice.13047)\
Audrey Olivier, Sevin Mohammadi, Andrew W. Smyth, Matt Adams, Computer‐Aided Civil and Infrastructure Engineering.


2022
----
1. [**"Data analytics for improved closest hospital suggestion for EMS operations in New York City"**](https://www.sciencedirect.com/science/article/pii/S2210670722004188)\
Audrey Olivier, Matt Adams, Sevin Mohammadi, Andrew Smyth, Kathleen Thomson, Timothy Kepler, Monish Dadlani, Sustainable Cities and Society, 2022.


2021
----
1. [**"Simulating NYC Hospital Load Balancing During COVID-19,"**](https://doi.org/10.1109/WSC52266.2021.9715419)\
   E. L. de Larrea, H. Lam, E. Sanabria, J. Sethuraman, S. Mohammadi, A. Olivier, A. Smyth et al., IEEE: Winter Simulation Conference, 2021
2. [**"The Role of Drivers' Social Interactions in their Driving Behavior: Empirical Evidence and Implication for Traffic Flow,"**](https://doi.org/10.1109/WSC52266.2021.9715419)\
   Sevin Mohammadi, Ramin Arvin, Asad J Khattak, Subhadeep Chakraborty, Transportation Research Part F: Traffic Psychology \& Behavior, 2021.


{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
