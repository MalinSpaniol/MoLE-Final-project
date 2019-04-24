# MoLE-Final-project
Artificial neural network to model iterated learning in language evolution 

## Background
This is the final project for the course "Models of Language Evolution", offered by Michael Franke at the winter term 18/19 at the University of Osnabrück. In the scope of this final project, our aim is to model the evolution of language by implementing a neural network. The current work is inspired by the iterated learning approach of Simon Kirby and James R. Hurford (2002). In their paper, they introduce the idea of an iterated learning model, which explains the emergence of linguistic structure. 

Kirby and Hurford explain that from an evolutionary perspective it is interesting to understand how human language, in which each utterance carries a semantic content and conveyes information about its own construction, could have evolved. 
In this context, they explain that language emerges at the intersection of learning, cultural evolution and biological evolution (Kirby at al. 2008). With the term "biological evolution", they refer to learning and processing mechanisms with which humans have been equipped with. Biological evolution happens through changes in inhterited traits over successive generations. With the term "cultural evolution", they refer to changes which are not inherited by individuals of a population, but changes such as meaning shifts, or phonological and syntactic adjustments in rules of a language. 

The authors stress that the evolution of language depends on social learning and cultural transmission in addition to biological evolution. In the literature about language evolution, “cultural evolution” is often modeled by “iterated learning”. 

Previous studies using iterated learning in the context of language evolution demonstrated that transmitted behaviour become easier to learn and increasingly structured. This development is considered to be cumulative and non-intentional.

The Iterated Learning Model (ILM) which Kirby and Hurford introduce, aims to model the structure of the mapping from meanings to signals, and the other way around. Their ILM consists of the 4 components: meaning space, signal space, language-learning agents and language-using adult agents. 

During each iteration of the ILM, an adult agent is given a set of randomly chosen meanings to produce signals for. The resulting meaning-signal pairs are used as training data for another learning agent. The learning agent has a learning period and then becomes an adult himself. This process is repeated several thousand times, or until a stable point attractor in the dynamical system is reached (Kirby et al. 2008). 

In the mentioned paper (2008), Kirby and Hurford use simple neural networks in order to model learners and show the process of iterated learning.

Our aim is now to repuduce their study. We want to implement neural networks with the same network architecture as used in the paper and see if we can get to the same results. 

## Current work

