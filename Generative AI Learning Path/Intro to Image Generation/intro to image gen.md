# Introduction to Image Generation

In this article, we'll dive into an introduction to image generation. Specifically, we'll provide an introduction to diffusion models, a family of models that have recently shown tremendous promise in the image generation space. Image generation has long been a field of interest, with many interesting approaches such as variational autoencoders, generative adversarial models (GANs), and autoregressive models .

## Image Generation Approaches

- **Variational Autoencoders (VAEs):** These models encode images to a compressed size and then decode them back to the original size while learning the distribution of the data itself .
- **Generative Adversarial Models (GANs):** GANs pit two neural networks against each other. One neural network, the generator, creates images, and the other neural network, the discriminator, predicts if the image is real or fake. Over time, both networks improve, leading to more realistic fake images .
- **Autoregressive Models:** These models generate images by treating an image as a sequence of pixels. The modern approach with autoregressive models draws much of its inspiration from how large language models (LLMs) handle text .

## Diffusion Models

Diffusion models, the focus of this article, are one of the newer image generation model families. They draw their inspiration from physics, specifically thermodynamics. Although first introduced for image generation in 2015, it took a few years for the idea to take off. Since 2020, diffusion models have seen a massive increase in both the research and industry spaces. They underpin many state-of-the-art image generation systems today .

### Unconditioned and Conditioned Diffusion Models

Diffusion models show promise across different use cases :

- **Unconditioned Diffusion Models:** These models have no additional input or instruction and can be trained from images of a specific thing, such as faces, and learn to generate new images of that thing. Super resolution is another example of unconditioned generation, which enhances low-quality images .
- **Conditioned Generation Models:** These models give us things like text-to-image, where we can generate an image from a text prompt, image inpainting, and text-guided image-to-image where we can remove or add things and edit the image itself .

### How Diffusion Models Work

The essential idea behind diffusion models is to systematically and slowly destroy the structure in a data distribution through an iterative forward diffusion process. This process involves adding noise to an image iteratively. We then learn a reverse diffusion process that restores the structure in the data, yielding a highly flexible and tractable generative model of the data. In other words, we can add noise to an image iteratively, and then train a model that learns how to denoise the image, thus generating novel images .

## Imagen: Text-to-Image Diffusion Model

Imagen is a text-to-image diffusion model with an unprecedented degree of photorealism and a deep level of language understanding. It builds on the power of large transformer language models in understanding text and hinges on the strength of diffusion models in high-fidelity image generation. Imagen achieves a new state-of-the-art FID score of 7.27 on the COCO dataset, without ever training on COCO, and human raters find Imagen samples to be on par with the COCO data itself in image-text alignment .

### DrawBench: A Benchmark for Text-to-Image Models

To assess text-to-image models in greater depth, DrawBench, a comprehensive and challenging benchmark for text-to-image models, was introduced. With DrawBench, Imagen is compared with recent methods including VQ-GAN+CLIP, Latent Diffusion Models, and DALL-E 2, and human raters prefer Imagen over other models in side-by-side comparisons, both in terms of sample quality and image-text alignment .

## Limitations and Societal Impact

There are several ethical challenges facing text-to-image research broadly. Downstream applications of text-to-image models are varied and may impact society in complex ways. The potential risks of misuse raise concerns regarding responsible open-sourcing of code and demos. Additionally, the data requirements of text-to-image models have led researchers to rely heavily on large, mostly uncurated, web-scraped datasets. These datasets often reflect social stereotypes, oppressive viewpoints, and derogatory associations to marginalized identity groups .
