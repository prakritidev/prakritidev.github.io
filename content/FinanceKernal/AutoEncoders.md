---
title: '"AutoEncoders"'
draft: 
tags: []
---



## **Types of Autoencoders & When to Use Them**

| Autoencoder Type                    | Key Feature                                   | When to Use?                                    |
| ----------------------------------- | --------------------------------------------- | ----------------------------------------------- |
| **Vanilla Autoencoder (AE)**        | Simple encoder-decoder architecture           | Basic feature compression                       |
| **Denoising Autoencoder (DAE)**     | Adds noise to input and learns to reconstruct | Handles noisy financial data                    |
| **Sparse Autoencoder (SAE)**        | Enforces sparsity in latent representation    | Selects key features from many inputs           |
| **Variational Autoencoder (VAE)**   | Learns a probabilistic latent space           | Used in generative modeling, Bayesian inference |
| **Contractive Autoencoder (CAE)**   | Adds Jacobian penalty to prevent overfitting  | Useful for robust feature learning              |
| **Convolutional Autoencoder (CAE)** | Uses CNNs instead of MLPs                     | Ideal for time-series and structured data       |
| **Transformer Autoencoder**         | Uses attention mechanisms                     | Captures long-range dependencies in time-series |
| **LSTM/GRU Autoencoder**            | Uses recurrent layers (LSTM/GRU)              | Best for sequential data like market trends     |
