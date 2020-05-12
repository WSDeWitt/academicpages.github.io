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
---

This is a module on population genetic inference for [Genome Sciences 541](https://noble.gs.washington.edu/~wnoble/genome541/).

Population genetics is a powerful lens on the history of humans and other species, revealing a complex past of migration, replacement, admixture with archaic populations, and dramatic population size changes during and after range expansions.
Germline mutation events in the history of a population give rise to variant alleles, and demographic histories are recorded in patterns of allele segregation within and between current-day populations.

Genetic drift is shared when populations mix, and it is private during periods of isolation.
The strength of drift is universally influenced by the population size history, and population genetic methods for demographic inference—expressed in the framework of Kimura's diffusion or Kingman's coalescent—attempt to recover the effective population size history _N(t)_, where time _t_ is measured in years or generations before the present.

**Essay by Kelley Harris:** [Reading the genome like a history book](https://science.sciencemag.org/content/358/6368/1265.2.full)
{: .notice--info}

### What can we do with demographic inference?

- Reconstruct the natural history of our favorite species (if we can get our friends to sequence it)
- Elucidate evolutionary forces that act in general cases
- Species conservation efforts
- Richer null models for adaptive hypotheses
- Interactions of genomic variation and human health


Goals
---
- Our focus will be on inference of effective population size history _N(t)_ from population genomic data
- We will build on the coalescent theory you learned in Trevor's module, extending to the setting of recombining genomes
- We will learn how demographic history is encoded in modern genomic variation, and three statistical paradigms for recovering it:
 1. Allele frequency spectrum
 2. Distribution of shared haplotype lengths
 3. Coalescent HMM

<!-- [link a section](#overview) -->
