# SoK: Security and Privacy in Machine Learning (2018, Papernot et al.)

## Introduction
|           | Training | Inference |
|:---------:|:--------:|:---------:|
|  Privacy  | X | • membership attack<br>• training data extraction<br>• model extraction |
| Integrity | poisoning | adversarial examples |

## Training: Integrity
* Label manipulation, input manipulation

## Inference
* Black box vs white box attackers at inference: ![Diagram](/images/sokml_diagram.png)
* ![Table](/images/sokml_table.png)
* Finding adversarial examples: applying existing optimizers (e.g. L-BFGS), fast gradient sign method (by Goodfellow et al.), Gradient Aligned Adversarial Subspace method (estimates the dimensionality of the dense space formed by adversarial examples)
* Adversarial example transferability
* Membership attack: figures out if a specific data point was part of the training data of the model
* Training data extraction: extracts not necessarily specific data points but average representations of inputs
* Model extraction: extracts model parameters

## Defenses
* Robustness to distribution drifts (i.e. when training and test distributions differ via attacks): PCA-based, regularization, obfuscation/disinformation, gradient masking (but limited success), injecting adversarial examples to training
* Gradient masking can be limited: ![Gradient](/images/sokml_gradient.png)
* Learning and inferring with privacy: differential privacy, homomorphic encryption, PATE
* Fairness and accountability in ML: encoding a sanitized variant of the data to learn fair models (auspicious future but still dispersed), quantitative input influence / activation maximization (i.e. knowing which inputs matter and how)