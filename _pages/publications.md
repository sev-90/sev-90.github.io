---
layout: archive
title: "Papers"
permalink: /publications/
author_profile: true
---
2024
----
1. **Surrogate Modeling of Trajectory Map-matching in Urban Road Networks using Transformer Sequence-to-Sequence Model**\
Sevin Mohammadi, Andrew W. Smyth, Revision submitted to  IEEE Intelligent Transportation Systems.
2. **Dynamic penalty-based dispatching decision-making for improved EMS response in urban environments: a heuristic approach**\
Sevin Mohammadi, Audrey Olivier, Andrew W. Smyth, Submitted to Sustainable Cities and Socity.

2023
----
1. **Probabilistic prediction of trip travel time and its variability using hierarchical Bayesian learning**\
Sevin Mohammadi, Audrey Olivier, Andrew W. Smyth, Accepted in ASCE-ASME Journal of Risk and Uncertainty in Engineering Systems, Part A.
2. **Bayesian neural networks with physics-aware regularization for probabilistic travel time modeling**\
Audrey Olivier, Sevin Mohammadi, Andrew W. Smyth, Matt Adams, Accepted in Computer‐Aided Civil and Infrastructure Engineering.


2022
----
1. [**"Data analytics for improved closest hospital suggestion for EMS operations in New York City"**](https://www.sciencedirect.com/science/article/pii/S2210670722004188)\
Audrey Olivier, Matt Adams, Sevin Mohammadi, Andrew Smyth, Kathleen Thomson, Timothy Kepler, Monish Dadlani in Sustainable Cities and Society, 2022.


2021
----
1. 


{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
