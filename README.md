# Variational AutoEncoder (VAE) for generating new handwritten digits 

## Introduction 

I have been trying to explore the world of Machine Learning for the past month or so , and my curiosity has led me to **AE** and **VAEs**. I could not understand the theory entirely at one go hence I decided to **learn by doing** instead. The following is my **imperfect** attempt to make a VAE to generate handwritten digits trained on the MNIST dataset and also my understanding of VAEs.

<hr>

## My Understanding

If I were to explain how a VAE works , it would likey be like this. 

A VAE is a **two network architecture** , comprising of a encoder and a decoder . However unlike an AE , the encoder takes in a image (in my case) and gives out two numbers which are **mu , and log(var)**. 

These numbers mu and log(var) are actually the parameters of a **multivariate gaussian distribution on the latent space** . Basically assigning a probability to all the infinite number of vectors in the latent space , based on how likley that vector created the input image. 

The Decoder is feed samples sampled from the probability distribution using the previously calculated numbers (mu , log(var)). The Decoder reconstructs the image given the vectors .
The Decoder tells us **"If we pick this latent vector z, here’s the most likely image".**

The loss function we use tries to force two things , the reconstruction should look like the original image and forces the latent vectors for all images to resemble a standard gaussian . It ensures we can generate new images by sampling from a normal distribution.

## The effect of Latent Dimension Size

### 1. Latent dimension = 2


![The Latent space scatter plot](<latent_dim_2 - Copy.png>)

Latent space scatter plot . Since the dim is 2 , we can just plot it for other dims I have used TSNE to reduce it into 2D. 

The model is forced to compress many digit variations into a 2D space, leading to overlapping clusters and shared representations.

Here is a GIF when we move from one cluster to another . 

![GIF Latent_dims = 2](<GIF_latent_dims_2.gif>)

If you look closely we move from 2 -> 3-> 8 ->7 ->9 ->1 . 

Here are the generated digits 

![GEN DIGIT LATENT_DIMS =2](<outputs_latent_dims_2.png>)

It seems it is struggling with the digits other than 1 , 0 or maybe 9. I suppose thats because we dont have enough dimensions to convey information regarding the curved strokes for all the other digits.

----

### 2. Latent dimension = 64 

![Latent_dims = 64 2d tsne plot](<Latent_dim_64.png>)

There's more orgnisation here , better clusters compared to previous. As we have more dimensions that we can use to convey information about the digits. 

![latent_dims = 64 Outputs](<outputs_latent_dims_64.png>)

Clearer , but they don't look so good?? I think I have an answer to that , and I may be very wrong about this. Since we are (for generating new images) sampling from a standard gaussian and feeding it to the decoder , it is highly likely that the decoder has never seen such a vector. And I think it becomes more likely the more dimensions we have for a given decoder. 

![latent_dims = 64 output , close](<Latent_dim_64_sample_close.png>)

These look pretty good , because they were sampled from the probability distribution found during training. 

Here is the GIF showing transition from 2 to 1 

![latent_dims = 64 gif](<GIF_latent_dims_64.gif>)

Notice we don't pass through many digits in this one. 

![](<interp2.gif>)

## Effect of KL loss 

### 1. Beta = 0 

β = 0 turns a VAE into a deterministic autoencoder with excellent reconstruction but no meaningful latent space or generative ability with no constraint on the latent space geometry.

![beta=0 output sampling](<beta0_output.png>)

This was obtained after sampling the z vectors from the standard gaussian , but there is no pressure on our model to match that distribution. Hence we get garbage 

this was obtained when we sampled from the distribution given by the encoder on the training data 

![beta=0 output sampling close](<Beta0_output_close.png>)

### 2. very high Beta (10)

![beta=10 output](<beta10_output.png>)

when we increase beta to be >>1 , then we basically prioritize minimising KL loss over the reconstruction loss . Then the probability distribution for all images approx. equals the Normal Distribution . Encoder is forced to ignore input details


β controls how much the model cares about being a valid generative model versus a good reconstructor.

![beta=10 TSNE output](<Beta0_sne.png>)

Because of the loss of input details , the reconstructions look blury and there is lack in diversity of the generated outputs. As shown in this GIF

![beta=10 GIF](<GIF_BETA10.gif>)


Thanks to everyone who made it till the end.

***This post isn’t a showcase of a perfect project — it’s a snapshot of my learning process. Writing this helped me identify and work through the gray areas in my understanding of VAEs, and I wanted to share that journey honestly***













