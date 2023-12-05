# Backdoor Detection in BadNets: A Pruning Defense

Using a backdoored facial recognition neural network, this repository applies the pruning defense outlined in this paper:
[Fine-Pruning: Defending Against Backdooring Attacks on Deep Neural Networks](https://arxiv.org/abs/1805.12185)


# Contents
- [Dependencies](#Dependencies)
- [Dataset](#Dataset)
- [Folder Structure](#Folder-Structure)
- [Architecture](#Architecture)
- [Usage](#Usage)
- [Evaluation](#Evaluating-the-Pruning-Defense-Model-On-Test-set)
- [Results and Observations](#Results-and-Observations)

# Dataset
   1. Download the validation and test datasets from [here](https://drive.google.com/drive/folders/1Rs68uH8Xqa4j6UxG53wzD0uyI8347dSq?usp=sharing) and store them under `data/` directory.
   2. The dataset contains images from YouTube Aligned Face Dataset. We retrieve 1283 individuals and split into validation and test datasets.
   3. bd_valid.h5 and bd_test.h5 contains validation and test images with sunglasses trigger respectively, that activates the backdoor for bd_net.h5. 
   ![image](images/Data.png)


# Folder-Structure

```bash
├── lab3
  ├── data 
      └── cl
          └── valid.h5              // this is clean validation data used to design the defense
          └── test.h5               // this is clean test data used to evaluate the BadNet
      └── bd
          └── bd_valid.h5          // this is sunglasses poisoned validation data
          └── bd_test.h5           // this is sunglasses poisoned test data
  ├── models
      └── bd_net.h5
      └── bd_weights.h5
  ├── repaired_models_asc
      └── model_drop_2.h5
      └── model_drop_4.h5
      └── model_drop_10.h5
  ├── repaired_models_desc
      └── model_drop_2.h5
      └── model_drop_4.h5
      └── model_drop_10.h5
  ├── images                       // Directory contains Result Images
  └── architecture.py
  └── eval.py                      // this is the evaluation script
  └── Machine_Learning_for_CyberSecurity_Backdoor_Attacks.ipynb
└── Machine_Learning_for_CyberSecurity Report.pdf                  // Report
└── evaluate_repaired_net.py    // To evaluate the repaired model on Test set
└── README.md 
```
# Architecture
The state-of-the-art DeepID network serves as the basic deep neural network (DNN) for face recognition. It consists of two parallel sub-networks that feed into the final two fully connected layers after three shared convolutional layers. The [architecture.py](architecture.py) file contains the model's architecture. Below are a flow diagram and a synopsis of the model.
```
___________________________________________________________________________________
 Layer (type)                   Output Shape         Param #     Connected to                     
===================================================================================
 input (InputLayer)             [(None, 55, 47, 3)]  0           []                               
                                                                                                  
 conv_1 (Conv2D)                (None, 52, 44, 20)   980         ['input[0][0]']                  
                                                                                                  
 pool_1 (MaxPooling2D)          (None, 26, 22, 20)   0           ['conv_1[0][0]']                 
                                                                                                  
 conv_2 (Conv2D)                (None, 24, 20, 40)   7240        ['pool_1[0][0]']                 
                                                                                                  
 pool_2 (MaxPooling2D)          (None, 12, 10, 40)   0           ['conv_2[0][0]']                 
                                                                                                  
 conv_3 (Conv2D)                (None, 10, 8, 60)    21660       ['pool_2[0][0]']                 
                                                                                                  
 pool_3 (MaxPooling2D)          (None, 5, 4, 60)     0           ['conv_3[0][0]']                 
                                                                                                  
 conv_4 (Conv2D)                (None, 4, 3, 80)     19280       ['pool_3[0][0]']                 
                                                                                                  
 flatten (Flatten)              (None, 1200)         0           ['pool_3[0][0]']                 
                                                                                                  
 flatten_1 (Flatten)            (None, 960)          0           ['conv_4[0][0]']                 
                                                                                                  
 fc_1 (Dense)                   (None, 160)          192160      ['flatten[0][0]']                
                                                                                                  
 fc_2 (Dense)                   (None, 160)          153760      ['flatten_1[0][0]']              
                                                                                                  
 add (Add)                      (None, 160)          0           ['fc_1[0][0]',      
                                                                  'fc_2[0][0]']                   
                                                                                                  
 activation (Activation)        (None, 160)          0           ['add[0][0]']                    
                                                                                                  
 output (Dense)                 (None, 1283)         206563      ['activation[0][0]']             
                                                                                                  
======================================================================================
Total params: 601,643
Trainable params: 601,643
Non-trainable params: 0
______________________________________________________________________________________
```

## Introduction

Backdoor attacks pose a significant threat to the integrity of neural network classifiers. This project presents a defense mechanism against such attacks, specifically in neural networks trained for facial recognition tasks, leveraging a pruning-based strategy.


## Methodology

Our defense strategy involves iterative pruning of the neural network to eliminate the backdoor while retaining classification capabilities. This involves targeting specific layers and channels based on their activation values and ceasing when validation accuracy degrades beyond an acceptable threshold.

## Dataset Visualization

- **Clean Dataset**: Visualizations provide insights into the data the model is expected to classify correctly under normal circumstances.
- **Poisoned Dataset**: Demonstrates how the backdoor trigger (e.g., sunglasses) is inserted into the images, altering the network's output.


# Results and Observations
![image](images/Evaluation_metrics.png)
## Performance on Test dataset
| Test Dataset | Threshold = 2% | Threshold = 4% | Threshold = 10% |
|---|---|---|---|
| Clean Accuracy | `95.90%` | 92.29% | 85.54% |
| ASR | 100% | 99.98% | `77.208%` | 


# Observations

- Pruning in descending order of mean activations implies removing the most highly activated channels first, which are often integral to the network's output.
- The removal of critical channels can drastically impact the model's generalization capability, leading to diminished accuracy on clean inputs.
- This approach poses the risk of over-pruning, which entails the loss of crucial features necessary for the model's functionality.

# Conclusion

Our study explores a pruning-based defense against backdoor attacks in neural networks, emphasizing the need for strategic pruning that mitigates threats without significantly impairing the model's performance.

---

For more detailed information, refer to our full report included in this repository.
