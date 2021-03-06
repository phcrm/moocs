# Lecture Notes

## 1 - Fri Oct  2

- General
        - VAE vs GAN
        - Generative models learn to produce *realistic* examples
        - Discriminative models distinguish between classes (*fake vs real*)
- Status
        - GAN's performance is rapidly improving :fire:
        - Major companies use them
- Discriminator
        - **Discriminator** is a classifier
        - It learns the probability of class Y (real/fake) given features X: $P(Y|X)$
        - Probabilities are the feedback for the generator
- Generator
        - The **generator** produces fake data
        - Learns the probability of features X: $P(X|Y)$
        - Takes as input random noise for different outputs each time
- BCE / Loss
        - The **BCE** cost function has two parts (one for each class)
        - Close to zero when the label and the prediction are similar
        - Approaches $\infty$ when the label and the prediction are different
- Training
        - GANs train in an alternating fashion
        - Both models should be of equal level, otherwise the feedback isn't useful


## 2 - Sun Oct  4

- Activations
        - Non-linear and differentiable
        - Differentiable for backpropagation
        - Non-linear to approximate complex functions
- Common activation functions
        - ReLU: $max(0, z)$
        - Dying ReLU -> Leaky ReLU: $max(az, z)$ (commonly $a=0.1$)
        - Tanh -1:1 / Sigmoid 0:1 (vanishing gradients)
- Batch Normalization
        - smooths the cost function
        - reduces internal covariance shift (distribution of data changing)
        - speeds up learning
        - introduces learnable shift and scale factors
            - $\hat z = \frac{z - \mu_z}{\sqrt{\sigma^2 + \epsilon}}$
            - scale $\gamma$ + shift $\beta$ factor: $y = \gamma \hat z + \beta$
        - during test, the running statistics from training are used
        - frameworks take care of the statistics (BatchNorm layer)
- Convolutions
        - slide filter / matrix over image
        - edge filter ... / learn the filter for feature x
        - stride & padding, pool & upsampling
- Transposed / De-Convolutions
        - upsample input
        - learnable params
        - result often in checkerboard patterns ([example](https://distill.pub/2016/deconv-checkerboard/))-> upsample+conv


## 3 - Fri Oct  9

- Mode Collapse
        - Modes are the peaks in the distribution of features
        - Typical in real-world datasets
        - Happens when the generator gets stuck in one image (i.e. only generating one single image, which fools the disc)
- Problems with BCE
        - GANs try to make the real and generated distribution look similar
        - When the discriminator improves too much, the function approximated by BCE will contain flat regions
        - Flat regions on cost function = vanishing gradients
- Earth Mover's Distance
        - EMD is function of amount and distance
        - Doesn't have flat regions when the distributions are very different
        - Approximating EMD solves the problem associated with BCE
- Wasserstein Loss
        - W-loss looks very similar to BCE Loss
        - W-loss prevents mode collapse and vanishing gradient problems
- Condition on Wasserstein Critic
        - Neural network needs to be 1-L continuous
        - This ensures that W-loss is validly approximating EMD
- 1-Lipschnitz Continuity enforcement
        - Weight clipping and gradient penalty are ways to enforce 1-L continuity
        - Gradient penalty tends to work better


## 4 - Mon Oct 12

- Conditional Generation
        - Intuition
            - requires labeled datasets
            - examples can be generated for the selected class
        - Input
            - Noise + Class (usually one-hot)
            - Class is passed to Generator and Discriminator
            - Both the discriminator and generator receive the class information appended to their traditional inputs for conditional generation
            - Could use extra NN for classprediction on Discriminator's end
