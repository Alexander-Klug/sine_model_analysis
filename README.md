# Sine Model Analysis

This repository contains the code and analysis for predicting a sine function using a neural network. The experiments focus on understanding the effect of neural network depth and width on model performance, when the total number of trainable parameters is kept approximately constant. The experiments further explores the performance of the neural network both within the training range and outside of it.

## Objective
- **Within the Training Range:** Where the NN is trained with data sampled from a specific range (e.g., [-10, 10]).
- **Outside the Training Range:** Where the NN is tested on extrapolated data outside this range (e.g., [-15, -10) ∪ (10, 15]).

## Key Steps

1. **Data Generation**:
   - We generate a dataset of `num_samples` random values from a uniform distribution within a specified `test_range` (e.g., [-10, 10]).
   - The target values are computed as $y = \sin(b)$.

2. **Model Configuration**:
   - We explore different depths (number of layers) and widths (number of neurons per layer) of the NN. The total number of trainable parameters is kept roughly constant across these configurations to ensure a fair comparison.
   - The depth is varied from 1 to 10 layers, and the width is adjusted accordingly.

3. **Training and Evaluation**:
   - For each depth, the NN is trained multiple times (defined by `runtimes`) to account for variability in the training process.
   - The performance is evaluated using Mean Squared Error (MSE) both on the test set within the training range and on the test set outside the training range.

4. **Averaging Results**:
   - The MSE results for each depth are averaged across all runs to obtain a more stable estimate of the network’s performance.

5. **Visualization**:
   - The MSE results are plotted to compare the NN’s performance as a function of its depth.
   - Separate plots are generated for the performance within the training range and outside the training range.


## Understanding Trainable Weights and Scaling

### Trainable Weights in a Neural Network

The number of trainable weights in a neural network is determined by the connections between the layers. For a simple feedforward neural network with a single hidden layer:
- **Input to Hidden Layer Weights:** $\text{Number of weights} = \text{(Number of input nodes)} \times \text{(Number of hidden nodes)} $.
- **Hidden to Output Layer Weights:** \( \text{Number of weights} = \text{(Number of hidden nodes)} \times \text{(Number of output nodes)} \).
- **Biases:** Each neuron (except those in the input layer) also has a bias term, adding to the total number of trainable parameters.

For example, if the network has 1 input node, 100 hidden nodes, and 1 output node, the total number of trainable parameters (weights + biases) can be calculated as follows:

1. **Input to Hidden Layer:** \( 1 \times 100 = 100 \) weights.
2. **Biases in Hidden Layer:** \( 100 \) biases.
3. **Hidden to Output Layer:** \( 100 \times 1 = 100 \) weights.
4. **Bias in Output Layer:** \( 1 \) bias.

Thus, the total number of trainable parameters is \( 100 + 100 + 100 + 1 = 301 \).

### Scaling the Width as a Function of Depth

To keep the total number of trainable parameters roughly constant while increasing the depth of the network, the width of each layer needs to be adjusted. If we start with 1 hidden layer of 100 neurons, the total number of parameters is approximately 301.

When adding more layers (increasing depth):
- The width of each layer must decrease to maintain the same total number of parameters.
- The width \( n \) for a network with depth \( d \) (number of hidden layers) can be approximated as:

\[ n \approx \sqrt{\frac{\text{total parameters}}{(d - 1)}} \]

For example, if we increase the depth from 1 to 2 layers while keeping the total parameters constant at 301:
- The width for 2 hidden layers would be:

\[ n \approx \sqrt{\frac{301}{2 - 1}} = \sqrt{301} \approx 17 \]

As the depth increases, the width \( n \) continues to decrease according to this relationship.

### Implications for Neural Network Design
- **Deeper Networks:** While deeper networks can capture more complex features, their layers need to be narrower to keep the total number of trainable parameters constant.
- **Wider Networks:** Shallower networks can have wider layers, but beyond a certain point, the increase in width yields diminishing returns on performance, especially when the number of parameters is fixed.


## Google Colab Notebooks
- [Sine Function Model Analysis](sine_model_analysis.ipynb): This notebook contains the code for generating data, building and training the model, and evaluating the model's performance.

## Overview
The first part of the analysis explores how the neural network's performance changes as a function of depth and width, while keeping the total number of trainable parameters roughly constant.

The second part of the analysis demonstrates how the model's predictions evolve during training using a GIF that visualizes the model's predictions over time.

![Training Progress](training_progress_5_8.gif)
