---
title: "Crops disease type prediction with Convolutional Neural Network (CNN)"
excerpt: "This project stemmed from my participation in the [ICLR Workshop Challenge CGIAR: Computer Vision for Crop Disease](https://zindi.africa/competitions/iclr-workshop-challenge-1-cgiar-computer-vision-for-crop-disease/data) in 2020."
collection: portfolio
---

Crops disease type prediction with Convolutional Neural Network (CNN)

This project stemmed from my participation in the [ICLR Workshop Challenge CGIAR: Computer Vision for Crop Disease](https://zindi.africa/competitions/iclr-workshop-challenge-1-cgiar-computer-vision-for-crop-disease/data) in 2020. Its repostory can be accesses [here](https://github.com/sev-90/competition1-crops-disease). 


This project developes a machine learning model to classify crops as healthy, suffering from stem rust, or affected by leaf rust, to study the devastating impact of wheat rust on African crops. Using **Convolutional NeuralNetworks (CNNs)** and the **fastai** library, the model is trained on imagery data collected from Ethiopia, Tanzania, and public sources. 
![image](https://github.com/user-attachments/assets/2fd5f3bf-1136-4a2a-9ac5-f29bf558a015)

The training pipeline involves **data inspection, cleaning, augmentation, transfer learning with ResNet34, progressive image resizing, and model re-training**. Key techniques include identifying and correcting mislabeled images, augmenting the small dataset to enhance generalization, and progressively resizing images to improve accuracy.

The training process begins with inspecting 876 labeled samples and 610 unlabeled test samples, followed by data cleaning to remove or relabel misclassified images. **Data augmentation** applies transformations like **flipping, rotation, and lighting adjustments**. **Transfer learning** with a pre-trained **ResNet34** model enhances feature extraction, and **progressive image resizing** (from 128x128 to 256x256) improves performance. The model undergoes additional fine-tuning by retraining on samples with high prediction loss. The classifier’s accuracy is monitored through error rate metrics, achieving the best possible result using optimized hyperparameters. The final model achieves a log loss of 0.3758 on the test set. 

![image](https://github.com/user-attachments/assets/5e85f39b-6234-4d6d-920f-dc63db0cc4a5)



References

    1. https://zindi.africa/competitions/iclr-workshop-challenge-1-cgiar-computer-vision-for-crop-disease/data

    2. https://course.fast.ai/
