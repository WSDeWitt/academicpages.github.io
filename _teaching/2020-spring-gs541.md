---
title: "Genome Sciences 541: Population genetic inference"
collection: teaching
permalink: /teaching/2020-spring-gs541
date: 2020-06-02
location: "Seattle, WA"
author_profile: true
layout: single
toc: true
toc_label: "Contents"
toc_icon: "dna"
toc_sticky: true
---

This is a module on population genetic inference for [Genome Sciences 541](https://noble.gs.washington.edu/~wnoble/genome541/), Spring quarter 2020.

Overview
---

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
- Reconstruct the natural history of our favorite species (if we can get our friends to sequence it)
- Elucidate evolutionary forces that act in general cases
- Inform species conservation efforts
- Formulate richer null models for adaptive hypotheses
- Clarify interactions of human genomic variation and human health


### Goals
Our focus will be on how the effective population size history $N(t)$ shapes population genomic data.
Specifically, we will:
- extend the coalescent theory you learned in Trevor Bedford's module to accommodate recombining genomes
- build intuition using simple calculations and simulations
- learn about three statistical paradigms for inferring demographic history:
 1. Allele frequency spectra
 2. Distributions of shared haplotype lengths
 3. Coalescent HMMs


Lectures
---

### lecture 1 under construction
{: .notice--danger}

### lecture 2 under construction
{: .notice--danger}


Homework
---

Please submit either a Jupyter notebook for each problem, or scripts and output images and text files that are clearly named.

### Problem 1: coalescent variance

Suppose you are studying a diploid population with a given constant population size $N = 10,000$.
You sequence 10 loci from each of 50 diploid individuals, so $n = 100$ samples for each locus.
A "locus" is just a contiguous chunk of DNA sequence.
Assume these 10 loci are unlinked, and no recombination happens within each locus.
This means that there is an independent coalescent genealogy for each locus.
Also suppose that each locus is about the same size, so they share a common mutatation rate $\mu=0.0001$ mutations per locus per generation.

**1.1** _(1 point)_ What is the expected TMRCA for any given locus?
{: .notice--primary}

**1.2** _(1 point)_ What is the expected number of segregating sites $S$ arising in one locus? Assume the loci are large enough so that the infinite sites approximation is reasonable (i.e. the number of segregating sites is equal to the number of historical mutation events).
{: .notice--primary}

<div class="notice--primary" markdown="1">
**1.3** _(8 points)_ Based on our sequencing, we can measure $S$ for each locus.
Suppose that we find one unusually high diversity locus with $S=1000$.
Your collaborator suggests this high diversity is indicative of diversifying selection at this locus.
To assess this, you will estimate the probability that at least one of the 10 measured $S$ values would be at least this high (i.e. a $p$-value), under the null hypothesis that these are just outcomes from the neutral coalescent process.

You will estimate this $p$-value by simulation.
One round of simulation means simulating a value of $S$ for each of the 10 loci.
Across many rounds of simulation, we can ask what fraction have a locus with $S\ge1000$.

To simulate one value of $S$:
- Generate a sequence of exponential random variables for the intercoalescent times $T_i$, with $i=100,99,\dots,2$.
- For each realized intercoalescent time $T_i=t_i$, generate a Poisson random variable for the number of mutations, where the Poisson mean is $it_i\mu$ ($i$ lineages persisting for $t_i$ generations gives a total branch length of $it_i$ in this interval).
- Add up the number of mutations simulated in each intercoalescent interval; this is your simulated $S$

Do you agree with your collaborator?
</div>


**Hints:**

You don't need to simulate the topology of the coalescent trees, only the time until the first event, then until second event, ...
{: .notice--success}

<div class="notice--success" markdown="1">
To simulate exponential and Poisson random variables (rvs) in Python, first import
```python
from scipy.stats import expon, poisson
```
To simulate an exponential rv with with rate `r` use
```python
expon.rvs(scale=1/r)
```
To simulate a Poisson rv with mean `m` use
```python
poisson.rvs(m)
```

**Warning:** note the reciprocal rate parameter above (called a "scale" parameter) used in `scipy`'s implementation
\\[\nonumber{\rm scale}\equiv \frac{1}{\rm rate}\\]
{: .notice--warning}
</div>

<div class="notice--success" markdown="1">
Similarly, in R use
```R
rexp(1, r)
```
and
```R
rpois(1, m)
```
</div>


### Problem 2: inferring demographic history with a coalescent HMM

This problem leverages variability in coalescent time across regions of two haplotypes from a recombining population to estimate demographic history $N(t)$.

Download [this VCF file]({{ site.url }}/teaching/2020-spring-gs541/variants.vcf), which contains variant calls for a 10Mb chromosome from 100 diploid individuals ($n = 200$).

<div class="notice--info" markdown="1">
**VCF file format:**
The `POS` field indicates the (0-based) position of each variant in each chromosome.
The individuals are named `tsk_0	tsk_1 ...`, and their phased genotypes are indicated as

- `0/0`: homozygous for ancestral (i.e. non-mutant) allele
- `0/1` or `1/0`: heterozygous for derived (i.e. mutant) allele
- `1/1`: homozygous for derived allele

Assume all other sites in the chromosome are homozygous ancestral (`0/0`) for all individuals.
</div>

**2.1** _(2 points)_ Compute and Plot the sample frequency spectrum (SFS).
Use log scaling for both x and y axes.
Does this look (qualitatively) like the SFS for a constant population history?
Briefly explain why or why not.
{: .notice--primary}

**2.2** _(2 points)_ Compute the sequence divergence between the two haplotypes from individual `tsk_0` in 100 kB windows across the chromosome, and plot this.
Produce a similar plot for the first 10 individuals `tsk_0 ... tsk_9`
{: .notice--primary}

**2.3** _(6 points)_ Use the Pairwise Sequential Markov Coalescent (PSMC) algorithm (see software notes below) to infer the demographic history $N(t)$ from each of the first 10 individuals `tsk_0 ... tsk_9`.
Plot these 10 histories on the same axes.
Briefly describe the event in the history of this population that gave rise to the SFS distortion.
{: .notice--primary}


<div class="notice--danger" markdown="1">
**Software notes:**
[under construction]

</div>

<div class="notice--success" markdown="1">
**Did you know?:**
You can execute terminal commands (i.e. PSMC) in Jupyter notebook cells by beginning with the `!` character, e.g.
```
!echo foo > bar.txt
```
You can even loop them, mixing Python and terminal commands, e.g.
```
for i in range(10):
  !echo {i} >> bar.txt
```
Note the curly braces grab the python variable `i` for the terminal command
</div>

<div class="notice--info" markdown="1">
**Original PSMC paper:** H. Li, R. Durbin, Inference of human population history from individual whole-genome sequences. [Nature 475, 493–496 (2011).](https://www.nature.com/articles/nature10231)

There are several more cutting-edge coalescent HMM methods, such as MSMC and SMC++.
Some might call PSMC deprecated; I call it classic.

**Review of modern coalescent HMMs:**
J. P. Spence, M. Steinrücken, J. Terhorst, Y. S. Song, _Inference of population history using coalescent HMMs: review and outlook_. [Curr Opin Genet Dev 53, 70–76 (2018).](http://dx.doi.org/10.1016/j.gde.2018.07.002)
</div>
