# Part1-module-5
Neural Network Fundamentals and Training Behavior Analysis

                                                          
                                                          
                                                          Task 6
Q1 — What role do weights and biases play in the model?
**Weights** are the learnable scalar values assigned to each connection between neurons.  
During the forward pass, a neuron computes:
z = w₁x₁ + w₂x₂ + ... + wₙxₙ + b
output = activation(z)
```
Each weight `wᵢ` controls **how strongly** input feature `xᵢ` influences the neuron's output.  
A large weight means that feature has a major effect on the prediction; a weight near zero means the model ignores that feature.

**Biases** are additional learnable parameters added to the weighted sum *before* the activation function.  
They shift the activation threshold, allowing the model to represent patterns that do not pass through the origin — without biases, the model could only represent linear relationships that cross zero.
Together, weights and biases form the **entire stored knowledge** of the network. After training, they encode every relationship the model learned from the data. Backpropagation adjusts them step by step to minimise the loss function.
--------------------------------------------------------------------------------------------------------------------------------------------------------
Q2 — Why is an activation function required?

Without an activation function, every layer performs only a **linear transformation**:
output = W · input + b
Stacking multiple such layers is mathematically equivalent to a *single* linear transformation — no matter how deep the network. The entire model could only represent straight-line (or hyperplane) decision boundaries.
Activation functions introduce non-linearity, enabling the network to model complex, curved decision boundaries that real-world data requires.
---------------------------------------------------------------------------------------------------------------------------------------------------------
Q3 — What happens when learning rate is too high or too low?

When learning rate is :-
 **Too High** (lr = 0.01)  : Gradient updates overshoot the minimum. Loss oscillates or diverges. Model may memorise training data but generalise poorly — high val_loss (0.19). Near-perfect train accuracy, but recall stays low.

**Too Low** (lr = 0.0001)  : Convergence is very slow. Train accuracy reaches only 94% after 100 epochs — still learning. However, the gradual exploration of the loss surface found a region where more churners are detected: **best recall of 67%**.

**Just Right** (lr = 0.001) : Stable, smooth convergence. Train and val curves track each other without oscillation. Adam's adaptive mechanism partially compensates for a fixed LR being imperfect.
------------------------------------------------------------------------------------------------------------------------------------------------------------
Q4 — Did the model show signs of underfitting or overfitting?

**The baseline model shows mild overfitting:**

Only 25 churned customers appear in the training set. With so few examples, the network struggles to learn a robust representation of what "churned" means. It defaults to classifying almost everything as "retained" — giving high accuracy but near-zero churn recall.
In Experiment 3 , With lr = 0.0001 and 100 epochs, training accuracy peaked at 94% — the model was still converging and hadn't yet fully utilised the training data. Paradoxically, this "underfit" model achieved the **best churn recall (67%)**, because it didn't over-specialise on the majority class.
We can do following to mitigate overfitting in production :-
-Add **Dropout layers** (e.g., 0.3) after each Dense layer for regularisation.
-Collect more real churn data — 31 positive examples is genuinely insufficient for deep learning.
