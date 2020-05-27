---
title: "Genome Sciences 541: Population genetic inference"
collection: teaching
permalink: /teaching/2020-spring-gs541
type: "Module"
venue: "University of Washington, Genome Sciences"
date: 2020-06-02
location: "Seattle, WA"
---

Overview
===

This is a module on population genetic inference for [Genome Sciences 541](https://noble.gs.washington.edu/~wnoble/genome541/).

Population genetics is the study of genetic variation shared among individuals, populations and species.
It is a powerful lens on the history of humans and other species, revealing a complex past of migration, replacement, admixture with archaic populations, and dramatic population size changes during and after range expansions.
Germline mutation events in the history of a population give rise to variant alleles, and demographic histories are recorded in patterns of allele segregation within and between current-day populations.

**Essay by Kelley Harris:** [Reading the genome like a history book](https://science.sciencemag.org/content/358/6368/1265.2.full)
{: .notice--info}

_Genetic drift_ is the stochastic change in allele frequencies over time due to random outcomes of mutation and survival in a finite population.
Genetic drift is shared when populations mix, it is private during periods of isolation, and the strength of drift is universally influenced by the population size history.
Statistical methods for demographic inference—expressed in the framework of Kimura's diffusion or Kingman's coalescent—attempt to recover the effective population size history $N(t)$, where time $t$ is measured in years or generations before the present.

**Review by Brian Charlesworth:** [Effective population size and patterns of molecular evolution and variation](https://www.nature.com/articles/nrg2526)
{: .notice--info}

<!-- Check dis[^foo]
[^foo]: bar -->

In the simplest settings, _demographic inference_ means inferring the effective population size history $N(t)$ for a single population, and thus identifying important events such as bottlenecks and range expansions.
More complex models aim to jointly infer the histories of several populations, and include divergence, admixture, and migration between them.
Demographic inference is a rapidly growing area of computational biology.
The field is theoretically rooted in probabilistic population models that predate knowledge of the structure of DNA, but have become applicable---and worth improving---by the need to understand population-scale genome sequencing data.

**Review by Josh Schraiber and Josh Akey:** [Methods and models for unravelling human evolutionary history](https://www.nature.com/articles/nrg4005)
{: .notice--info}


What can we do with demographic inference?
---
- Reconstruct the natural history of our favorite species (if we can get our friends to sequence it)
- Elucidate evolutionary forces that act in general cases
- Inform species conservation efforts
- Formulate richer null models for adaptive hypotheses
- Clarify interactions of human genomic variation and human health


Goals
---
Our focus will be on how the effective population size history $N(t)$ shapes population genomic data.
Specifically, we will:
- extend the coalescent theory you learned in Trevor's module to accommodate recombining genomes
- build intuition using simple calculations and simulations
- learn about three statistical paradigms for inferring demographic history:
 1. Allele frequency spectra
 2. Distributions of shared haplotype lengths
 3. Coalescent HMMs

<!-- [link a section](#overview) -->
