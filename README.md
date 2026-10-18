[🇬🇧 English](README.md) | [🇮🇹 Italiano](README.it.md)

# BoletusSpieces — What Type of Boletus Is It? 🍄

An experimental **image classification project** built with Python, fast.ai, and PyTorch.

The goal is to explore how transfer learning can be used to distinguish visually similar mushroom species using photographs collected from the web.

This project was inspired by the first lesson of [Practical Deep Learning for Coders](https://course.fast.ai/Lessons/lesson1.html).

> **Disclaimer:** This is an educational experiment, not a reliable mushroom identification tool. Predictions must never be used to determine whether a mushroom is safe to eat.

## 1. Project Overview

The project follows a complete machine learning workflow:

1. Collect mushroom photographs using automated web searches.
2. Organize the images into species-specific directories.
3. Validate and preprocess the dataset.
4. Train an image classifier using a pretrained neural network.
5. Evaluate the model's performance.
6. Make predictions on unseen images.

### Supported Species

| Species               | Class label           |
| --------------------- | --------------------- |
| *Boletus aereus*      | `Boletus_aereus`      |
| *Boletus edulis*      | `Boletus_edulis`      |
| *Boletus pinophilus*  | `Boletus_pinophilus`  |
| *Boletus reticulatus* | `Boletus_reticulatus` |

The classifier is currently limited to these four species.

## 2. Technologies

The project uses:

- **Python 3.12** — Main programming language
- **fast.ai** — Deep learning framework
- **PyTorch** — Neural network backend
- **DDGS** — Automated image search
- **fastcore** — Data manipulation utilities
- **Jupyter Notebook** — Experimentation and documentation

## 3. Dataset Collection

Images are collected automatically using DDGS.

For each mushroom species, the program generates a search query and retrieves image URLs. The photographs are downloaded into separate directories using fast.ai's `download_images()` function.

Images are then resized and checked for corrupted files.

**Dataset limitations:** Web searches do not guarantee accurate species identification. Incorrect labels, duplicate photographs, and inconsistent image quality may affect model performance.

The downloaded dataset is not included in this repository. Image copyrights and licenses remain with their respective owners.

## 4. Data Preparation

The dataset is prepared using fast.ai's `DataBlock` API.

Images are assigned labels based on their parent directory.

The data is randomly divided into:

- **80% training set**
- **20% validation set**

A fixed random seed (`seed=42`) is used to make the split reproducible when the dataset remains unchanged.

Images are resized to 192 × 192 pixels using padding to preserve their proportions.

## 5. Model Training

The classifier uses **ResNet18**, a convolutional neural network pretrained on a large image dataset.

Instead of training the network from scratch, the project uses transfer learning to adapt its learned visual features to mushroom classification.

The model is trained using fast.ai's `fine_tune()` method, with five fine-tuning epochs.

### Training Configuration

| Parameter          | Value             |
| ------------------ | ----------------- |
| Architecture       | ResNet18          |
| Learning approach  | Transfer learning |
| Fine-tuning epochs | 5                 |
| Image resolution   | 192 × 192         |
| Batch size         | 16                |
| Validation split   | 20%               |
| Evaluation metric  | Error rate        |

## 6. Initial Results

The initial experiment produced the following validation results:

| Metric                 |     Result |
| ---------------------- | ---------: |
| Validation images      |         29 |
| Correct predictions    |         13 |
| Accuracy               | **44.83%** |
| Error rate             | **55.17%** |
| Lowest validation loss | **1.3796** |

### Training Observations

The training loss decreased consistently, indicating that the model was learning patterns from the training images.

However, validation loss stopped improving during the early fine-tuning epochs.

This suggests possible overfitting, although additional investigation is necessary because the dataset and validation set are relatively small.

### Confusion Matrix

The confusion matrix provides additional information about the species the model struggles to distinguish.

One particularly notable result concerns *Boletus edulis*: none of its six validation images were classified correctly.

These results highlight the importance of improving dataset quality before experimenting with more complex architectures.

## 7. Model Inference

After training, the classifier can make predictions on a new photograph using fast.ai's `learn.predict()` method.

The prediction includes:

- The predicted mushroom species.
- The numerical class index.
- The probability scores for each class.

These scores do not guarantee that the predicted species is correct.

## 8. Current Limitations

This experiment has several important limitations:

- The dataset was collected through general image searches.
- Species labels have not been systematically verified.
- The validation set is small.
- Duplicate or near-duplicate photographs may affect evaluation.
- The model has not been evaluated on a separate, curated test dataset.
- Unknown mushroom species may be incorrectly classified as one of the four supported classes.

## 9. Future Improvements

Potential next steps include:

- Expanding the dataset with reliably identified photographs.
- Removing duplicates and correcting labels.
- Improving the training and validation split.
- Applying data augmentation techniques.
- Comparing different neural network architectures.
- Measuring precision and recall for individual species.
- Exporting the trained model.
- Developing a demonstration interface.

## 10. Conclusion

This project represents my first independent experiment with image classification using fast.ai.

Although the initial results show that the classifier requires significant improvement, the project has provided practical experience with dataset preparation, transfer learning, model evaluation, and error analysis.

The main objective is not to achieve perfect predictions immediately, but to understand how data quality, training choices, and evaluation methods affect the performance of a deep learning model.

---

**Project Status:** `v0.1 — Experimental Prototype`