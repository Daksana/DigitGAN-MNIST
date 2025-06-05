# DigitGAN: Generating and Classifying MNIST Digits with GANs

## Overview

This project implements a Generative Adversarial Network (GAN) to generate handwritten digit images similar to the MNIST dataset. It also includes a classifier model to evaluate real and generated digits, and performs analysis to understand classifier behavior on real (S0) and generated (S1) test sets.

## Files

* `GAN_for_MNIST.ipynb`: Complete training and evaluation code for the Generator, Discriminator, and Classifier.
* `Fake_Digits/`: Folder structure where 100 GAN-generated digits were saved, organized by label.
* `fakeimage_grid.png`: A 10x10 grid visualization of selected GAN-generated digits.
* `c.pkl`: Saved PyTorch classifier model.
* `G.pkl`, `D.pkl`: Saved Generator and Discriminator model weights.

## GAN Model

* **Architecture:** A standard GAN with a Generator and Discriminator trained on MNIST (28x28 grayscale digits).
* **Generator Input:** 100-dimensional random latent vector `z`.
* **Generator Output:** 28x28 grayscale digit image.
* **Loss Function:** Binary Cross-Entropy for both Generator and Discriminator.
* **Training Data:** MNIST training set (60,000 images).
* **Epochs:** 50 epochs.
* **Improvements:**

  * Used LeakyReLU activations.
  * Batch normalization in Generator.
  * Normalized image pixels to \[-1, 1].
  * Regular monitoring of generated image quality.

## Classifier Model

* **Architecture:** Fully connected neural network.
* **Training Data:** MNIST training set (60,000 images).
* **Testing Data:** MNIST test set (10,000 images).
* **Epochs:** 5
* **Final Training Accuracy:** 99.44%
* **Testing Accuracy:** 98.98%

## Results

### 10x10 Grid of Generated Digits

* ![Generated Digits](fakeimage_grid.png)

### Classifier Accuracy

| Epoch | Loss   | Accuracy |
| ----- | ------ | -------- |
| 1     | 142.94 | 95.36%   |
| 2     | 40.91  | 98.67%   |
| 3     | 28.10  | 99.04%   |
| 4     | 21.00  | 99.27%   |
| 5     | 15.64  | 99.44%   |

### Classification Performance

* **S0 (Real Test Set):** 0.00% error
* **S1 (Generated Set):** 18.00% error

## Observations

* Classifier performs very well on real MNIST data (0.00% error on S0).
* Error on generated images (S1) is 18.00%, suggesting some digits lack visual clarity.
* Indicates challenge in GAN learning diverse and high-quality digit patterns.

## Final Question Response

> **“How would you generate specific digits using your existing Generator?”**

To generate specific digits (e.g., image of 5), we propose training a **Latent Code Predictor** (LCP):

1. **Input:** One-hot encoded digit label.
2. **Output:** 100-dimensional latent vector `z`.
3. **Generator G:** Frozen; receives z from LCP and generates an image.

**Training Process (Suggested):**

* Generate many `z` samples.
* Pass through Generator → fake images.
* Classify images using the trained Classifier.
* Keep (label, z) pairs where the prediction is correct.
* Train the LCP to map label → z using MSE loss.

**Note:** This is a proposed extension and is not implemented in the code.

