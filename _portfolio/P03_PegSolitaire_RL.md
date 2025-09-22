---
title: "Solving the game of peg solitaire with a Reinforcement Learning"
excerpt: "This project was completed as part of my Deep Learning course's final group project in 2020."
collection: portfolio
abstract: "This project applies an adapted version of Asynchronous Advantage Actor-Critic ([A3C](https://arxiv.org/pdf/1602.01783.pdf)), implemented from scratch, to train an RL agent to solve the **peg solitaire** game. The game consists of **32 marbles (pegs)** arranged in a cross shape on a **33-position** board, with the center position initially left empty. The objective is to remove marbles one by one until only one remains. A marble is removed when another marble jumps over it into an empty space."
---



## RL-solitaire

<!--This project applies an adapted version of Asynchronous Advantage Actor-Critic ([A3C](https://arxiv.org/pdf/1602.01783.pdf)), implemented from scratch, to train an RL agent to solve the **peg solitaire** game. The game consists of **32 marbles (pegs)** arranged in a cross shape on a **33-position** board, with the center position initially left empty. The objective is to remove marbles one by one until only one remains. A marble is removed when another marble jumps over it into an empty space.-->

The following GIF illustrates how the game is played:

<p align="center">
<img src="/images/solitaire_1.gif" width="400" height="400" />
</p>

It is relatively easy to end the game with 2–5 marbles remaining, but achieving a single remaining marble is significantly more difficult. This makes peg solitaire challenging for reinforcement learning since the agent may learn to maximize rewards by reaching a near-complete solution but must reach exactly one remaining marble to fully solve the game.


## Method Overview

A modified version of A3C is used, where 16 games run simultaneously, sharing the same agent (policy network). Game data is stored in a memory buffer containing:

- State representation
- Advantage estimate
- Selected action
- Critic target (value function update)

Every 4 moves, sampled data is used to update the policy-value network using mini-batch gradient descent (batch size: 64, optimizer: Adam). A training iteration consists of: 

1. Running all 16 games to completion
2. Updating the network after every 4 moves by all 16 agents
3. Saving the updated model and running 30 evaluation games 

## Neural Network Architecture 

The state representation is encoded as a 7×7×3 input tensor: 

- Channel 1: Binary values indicating marble presence (1) or absence (0)
- Channel 2: Percentage of marbles removed so far
- Channel 3: Percentage of marbles left to remove

The policy-value network consists of: 

1. Three 2D convolutional layers for state encoding
2. Two separate network heads:
   - Value head: 1×1 convolution → Dense layer → Output layer
   - Policy head: 1×1 convolution → Dense layer → Softmax logits

At each game state, the network computes: 

- Critic target using cumulative rewards and bootstrapping from future values
- Advantage estimates for policy updates
- Selected action storage for actor training

## Training and Results 
With the given configuration, training took 53 minutes on a single CPU for 800 iterations. By iteration 700, the agent solved the puzzle 99% of the time during evaluation. The agent: 

- Reliably solves the puzzle when sampling from the policy
- Always finds a solution when using a greedy strategy (choosing the most probable move at each step)
- Learns to solve the game after ~11,000 training games (~50,000 network updates)

Below are plots showing training performance:

<p align="center">
  <img src="/images/rewards_1.jpeg" width="440" height="350" title="Reward as a function of the number of iterations" />
  <img src="/images/pegs_left_1.jpeg" width="440" height="350" title="Number of marbles left as a function of the number of iterations" />
</p>


## Final Solution Demonstration 

The following GIF showcases the trained agent solving the puzzle using a greedy policy (always selecting the most probable move):


<p align="center">
<img src="/images/solitaire_opt_trim_1.gif" width="400" height="400" />
</p>

## Improvements & Future Work 

Although a simple network architecture was used, a more complex design could improve learning speed. Additional enhancements, such as curriculum learning or reward shaping, could further improve agent performance.
