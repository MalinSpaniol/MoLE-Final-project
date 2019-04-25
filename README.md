# MoLE-Final-project
### Sophia Schulze-Weddige & Malin Spaniol WS 18/19
Artificial neural network to model iterated learning in language evolution 

## Background
This is the final project for the course "Models of Language Evolution", offered by Michael Franke at the winter term 18/19 at the University of Osnabrück. In the scope of this final project, our aim is to model iterated learning by implementing a neural network. 

The current work is inspired by the iterated learning approach of Simon Kirby and James R. Hurford (2002), Smith (2014), Kirby and Smith (2014) and Smith (2003). In the different papers, they make use of the implementation of neural networks in order to model iterated learning. The network structure of the different papers varies slightly, however, the idea behind it is the same. For the current work, we finally decided to focus mainly on the paper of Kirby and Hurford (2002). 

Kirby and Hurford explain that from an evolutionary perspective it is interesting to understand how human language, in which each utterance carries a semantic content and conveyes information about its own construction, could have evolved. 
In this context, they explain that language emerges at the intersection of learning, cultural evolution and biological evolution (Kirby at al. 2008). With the term "biological evolution", they refer to learning and processing mechanisms with which humans have been equipped with. Biological evolution happens through changes in inhterited traits over successive generations. With the term "cultural evolution", they refer to changes which are not inherited by individuals of a population, but changes such as meaning shifts, or phonological and syntactic adjustments in rules of a language. 

The authors stress that the evolution of language depends on social learning and cultural transmission in addition to biological evolution. In the literature about language evolution, “cultural evolution” is often modeled by “iterated learning”. 

Previous studies using iterated learning in the context of language evolution demonstrated that transmitted behaviour become easier to learn and increasingly structured. This development is considered to be cumulative and non-intentional.

The Iterated Learning Model (ILM) which Kirby and Hurford introduce, aims to model the structure of the mapping from meanings to signals, and the other way around, so to say, it explains the emergence of linguistic structure. Their ILM consists of the 4 components: meaning space, signal space, language-learning agents and language-using adult agents. 

During each iteration of the ILM, an adult agent is given a set of randomly chosen meanings to produce signals for. The resulting meaning-signal pairs are used as training data for another learning agent. The learning agent has a learning period and then becomes an adult himself. This process is repeated several thousand times, or until a stable point attractor in the dynamical system is reached (Kirby et al. 2008). 

In the mentioned paper (2008), Kirby and Hurford use simple neural networks in order to model learners and show the process of iterated learning.

Our aim is now to repuduce their study. We want to implement neural networks with the same network architecture as used in the paper and see if we can get to the same results. 

## Current work

### The simple neural network of Kirby and Hurford 
The simple iterated learning model of Kirby and Hurford (2002) uses one neural network for each language learning agent. As the model is very simplified, one agents exemplifies one whole generation. One agents consists of a feed-forward networks with 8x8x8 structure, meaning that we have an input signal of 8 neurons, a hidden layer with 8 neurons and an output layer with 8 neurons. All neurons are fully connected.  
Each network maps from 8bit binary numbers representing a signal to 8-bit binary numbers, representing a meaning. So to say, given an appropriate input, the agents can learn to parse utterances. 

Each network is unidirectional (therefore we need some production mechanism to map from meaning back to signals). For this, the authors assume the obverter strategy (2002). The underlying assumption is that the speaker's own mapping is similar to the hearer's one. The signal producing agent produces the signal which maximizes the chance that the hearer can understand the correct meaning. 

In the first population we initialize to networks (speaker & hearer). A certain number of random meanings is chosen from the set of binary numbers 00000000 to 11111111 (256 possible meanings). The weights of both networs are initialized randomly. For the fixed number of signals produced (number of utterences, which makes up the bottleneck) the meanings are at first (in the first speaker network) assigned randomly. After that, the speaker produces signals for each meaning, by applying obverter procedure. 
For this, training phase and production phase of agent are separated, and the network weights will be fixed throughout the production phase. The results, so the meanings produced by an agent, as a response the the signals, are stored in lookup table. 

The set of signal-meaning pairs is used to train the hearer network using backpropagation of error learning algorithm, with a learning rate of 0.1, and no momentum term. 

Each learner is trained for 100 epochs. After that, the speaker is removed, and the previos hearer is the new speaker. A new hearer is added, which means that a new network is created with randomly initialized weights. The number of how often this cycle is repeated depents on the number of generation which should be modeled. In the paper, this number varies between 50 and 250. The size of the training set (nr. of utterences from which each network learns) also varies. In the figures, the authors show graphs for randomly chosen 20, 50, and 2000 meanings. 

For visualization, the authors show graphs for the expressivity and stability of the language over generations. This is represented by the difference between learners' asd adults' language after training and the meaning space which is covered by the language.

The authors claim that if the number of training exaples is "not too small and not too large (the particular numbers seem to depend on the structure of the networks, and the size of the meaning and singal spaces - see Brighton & Kirby (2001a;2001b) for an approach to quantifying the crifical values for the ILM)", a completely stable and completely expressive language is found. 

### Extras of our network

All given information about the neural network architecture was implemented by us. At some points, insufficient information was given about the implementaion. Therefore, some additional decisions were made. The code in the project_spaniol_schulzeweddige.ipynb file is commented in a way, that all decisions are explained at every step, when they were made.

However, here we will summarize the most important decisions, too:

We decided to cut the training data into batches, in order to improve the time needed for computation and to avoid local minima. However, this should not have a significant effect on the results. 

Concerning the model, there was no information given about which activation functions were used for the output layer, nor for the hidden layer. Therefore, we chose to use a sigmoid function for the output layer. The weights in the output layer are randomly initialized using the He initializer, which depends on the number of neurons of the previous layer (see further explanation in the code). For the hidden layer, we use the tangens hyperbolicus, as it is a frequently used actiavtion function. The weights in the hidden layer are initialized using the Xavier initializer, which again depends on the number of neurons in the previous layer (or in our case the number of input neurons). The paper doesn't give a any information about the initialization of the biases, therefore we use the golden standard and initialize the biases with zeros.

In the paper it is not mentioned specifically what loss was used. We now use the Sigmoid Cross-Entropy loss, which is a Sigmoid activation plus a Cross-Entropy loss. Unlike Softmax loss, it is independent for each vector component (class), meaning that the loss is computed for every output vector component and is not affected by other component values. That’s why it is used for multi-label classification, were the insight of an element belonging to a certain class should not influence the decision for another class. In our case the classes are 0 and 1 and we want to decide for each position in the 8-bit vector, whether it should be 1 or a 0.

Kirby & Hurford mention that no momentum is used and that a learning rate is 0.1. This was kept, the optimizer we chose is a simple gradient descent.  


## Evaluation of results

As a measure of evaluation, we used the expressivity, considered the covered meaning space and the stability of the language, considered mean squared difference of signal-meaning-pairs between two consecutive generations.

We use the size of the meaning space as a measure for the expressivity of the learned language of each generation. It is calculated by counting the number of different meanings in the output table. For this, we plotted a graph for 20, 50 and 2000 utterances, (such as in the paper), showing the percentage of covered meaning space across the number of generations.
Further, we calculate the mean squared difference between the output tables giving the signal-meaning pairs of two consecutive generations, as a measure for stability of the language. Again, we plotted three graphs (for 20, 50 and 2000 utterances), which can be found in the Graphs folder. 

All six graphs visualize, that there are huge fluctuations in our results, for all three cases ( 20, 50 and 2000 utterances). This means, that the covered meaning space and the mean squared difference between generations both fluctuate a lot. 

The authors showed in their study, that eventually, at some point, if the number of training exaples is "not too small and not too large" (Kirby & Hurfod 2002), a completely stable and completely expressive language is found. Our implementation did not reach this point. We did not reach a stable point at all, and in the most expressive generation, only 40% of the meaning space was covered. 

We don't know why this is so, but the information about the implementation of the authors is quite vague, which leaves a lot of space for differences in the code. We played a lot around at different points in the implementation, such as the activation functions, different measures for the standard deviations for the initialization of the random weights, normalization of the input or not, using differnt measures for calculating the difference between generations and so on. However, this is the best result which we could reach. From the calculation of the loss, we can see that each network/agent is able to "learn", as the loss values always start around 2 and sometimes go down up to 0.2. Also the percentage of meaning space covered varies a lot, dependent on the previously mentioned changes. Still, we did not succeed to reach a completely stable and completely expressive language, such as we have hoped to. 
