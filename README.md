# Backdoor Detection and Mitigation in Neural Networks Using Pruning Defense

This repository is dedicated to the detection and mitigation of backdoor attacks in neural networks, with a focus on facial recognition systems. Our approach utilizes a novel pruning defense mechanism to counteract these attacks, as outlined in our comprehensive report.

## Contents

- Introduction
- Methodology
- Dataset Visualization
- Results
- Graphical Analysis
- Evaluation on Test Datasets
- Discussion
- Observations
- Conclusion

## Introduction

Backdoor attacks pose a significant threat to the integrity of neural network classifiers. This project presents a defense mechanism against such attacks, specifically in neural networks trained for facial recognition tasks, leveraging a pruning-based strategy.

[lab3]/
├── data/
│   ├── clean/
│   │   ├── valid.h5
│   │   └── test.h5
│   └── bd/
│       ├── bd_valid.h5
│       └── bd_test.h5
├── models/
│   ├── bd_net.h5
│   ├── bd_weights.h5
├── repaired_models_asc/
│   ├── model_drop_2.h5
│   ├── model_drop_4.h5
│   ├── model_drop_10.h5
├── repaired_models_desc/
│   ├── model_drop_2.h5
│   ├── model_drop_4.h5
│   ├── model_drop_10.h5
├── architecture.py
├── backdoor_detector.py
├── Report.pdf
├── README.md
└── Machine Learning for CyberSecurity Backdoor Attacks.ipynb


## Methodology

Our defense strategy involves iterative pruning of the neural network to eliminate the backdoor while retaining classification capabilities. This involves targeting specific layers and channels based on their activation values and ceasing when validation accuracy degrades beyond an acceptable threshold.

## Dataset Visualization

- **Clean Dataset**: Visualizations provide insights into the data the model is expected to classify correctly under normal circumstances.
- **Poisoned Dataset**: Demonstrates how the backdoor trigger (e.g., sunglasses) is inserted into the images, altering the network's output.

## Results

We document the initial performance of the BadNet model and the impact of our pruning process, which indicates a trade-off between maintaining accuracy on clean data and reducing the attack success rate.

## Graphical Analysis

A comprehensive graphical analysis illustrates the effectiveness of our pruning defense, showing the stability of classification accuracy on clean data against the fraction of pruned channels and the corresponding decrease in attack success rate.

## Evaluation on Test Datasets

Our evaluation demonstrates the GoodNet model's ability to maintain high classification accuracy while significantly reducing the attack success rate as more channels are pruned.

## Discussion

Our experiments reveal that pruning is a viable strategy for mitigating backdoor attacks, balancing accuracy and security. We discuss the implications of our findings and the potential for pruning as a foundational defense mechanism in neural network security.

## Observations

- Pruning in descending order of mean activations implies removing the most highly activated channels first, which are often integral to the network's output.
- The removal of critical channels can drastically impact the model's generalization capability, leading to diminished accuracy on clean inputs.
- This approach poses the risk of over-pruning, which entails the loss of crucial features necessary for the model's functionality.

## Conclusion

Our study explores a pruning-based defense against backdoor attacks in neural networks, emphasizing the need for strategic pruning that mitigates threats without significantly impairing the model's performance.

---

For more detailed information, refer to our full report included in this repository.
