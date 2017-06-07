---
title: "Shiny Apps of Lotka Volterra Models"
date: "2017-06-07T21:48:51-07:00"
---

## Introduction

The study of prey-predators interactions is a very exciting field, where a wide variety of models is available. The most famous is obviously that of Lotka-Volterra created in parallel by Vito Volterra and James Lotka (V. Volterra, Nature, 118: 1926). This model contains shortcomings such as the infinite growth of prey, which is not realistic (Malthus growth rate). It is indeed more accurate to use a Verhulst growth function, which takes into consideration competition between preys. Since then, a lot of model were developed, with different functional responses, each characterising prey-predators interactions: Holling 1, Holling 2 (Rosensweig-MacArthur), Holling-Tanner, Beddington... The following app aims at providing a comprehensive tool to investigate the dynamics of such systems. Basic phase plane analysis is included: equilibrium points, stability (but not the nature: focus, node, star ...). For the moment, Lyapunov functions or more sophisticated tools are not present. Thus a lot of mathematical features will be implemented as soon as possible. Besides, local and global sensitivity analysis can be tested with the first "basic Lotka Volterra model". Finally, only positive equilibriums are considered, for biological consistency.

<iframe src="https://dgranjon.shinyapps.io/lotka_volterra_bdd/" style="width: 700px; height: 700px; border: none; overflow: hidden;"></iframe>

Last update: 07/06/17