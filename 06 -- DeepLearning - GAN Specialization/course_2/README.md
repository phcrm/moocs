# Lecture Notes

## 1 - Thu Oct 29 (Evaluation of GANs)

- Challenge: A discriminator cannot be used for evaluation because it overfits to the generator it's trained with.
	- Two important properties
		- Diversity: variety of images
		- Fidelity: quality or realism of the image
- Pixel Distance
	- Bad!
	- Example: swap to two rows, small difference but huge pixel distance
- Feature Distance
	- Sum feature differences
	- Example: 1 feet, 1 nose vs 5 feet, 3 eyes
- Feature Extraction
	> ![Equation FID](img/image_2020-10-29-12-19-49.png)
	- Use pretrained classifier (ex. on ImageNet)
	- Not last / fully connected layer but feature layer (last pooling layer)
	- Use earlier layer if you need lower-level features (or if NN was trained on too small problem set)
	- Commonly used: Inception-v3
	- These Features are also called embeddings
	- Compare embeddings to compute Feature Distance
- Frechet Inception Distance -- $FID$
	- Calculates difference between reals and fakes
	- Use embeddings as two distributions and do statistics on those
	- Lower = Better
	- In `multivariate` distribution
	- Use large sample -> which makes it slow to run
- Inception Score ([Paper](https://arxiv.org/abs/1801.01973))
	> Original: ![inception-score](img/inception-score.png)
	> Improved: ![inception_improved](img/inception_improved.png)
	- Use classification (last layer)
	- KL Divergence
	- $1 \leq IS(G) \leq 1000$
		- $1000 = #classes$
		- Higher = Better
	- Can be exploited
		- generate only one of each class
		- highly overfitting
	- Only looks at fakes not reals
	- Slightly different trained NN can lead to drastic metric changes, even if accuracy is equal (Keras vs Torch Inception3)
	- Can miss important features
		- ImageNet isn't everything
	- Mostly replaced by FID
- Sampling and Truncating
	- Sampling $z$ at $0$ leads to great fidelity but poor diversity
	> ![truncation_trick](img/truncation_trick.png)
	> It truncates the normal distribution that you sample your noise vector from based on a hyperparameter that determines how much of the tails to cut off or keep.
	> If you want higher fidelity, you want to sample around 0 and truncate a larger part of the tails. If you want greater diversity, then you want to sample more from the tails of the distribution and have a lower truncation value.
	- can lead to worse FID
	- HYPE (Human eYe Perceptual Evaluation) [(Paper)](https://arxiv.org/abs/1904.01121)
- Recall & Precision [(Paper)](https://arxiv.org/abs/1904.06991)
	> $g=fake, r=real$
	> ![precision](img/precision.png)
	> $$p = \frac{overlap}{all fakes}$$
	> ![recall](img/recall.png)
	> $$r = \frac{overlap}{all reals}$$
	- G tends to cover all **and more**
		- good recall, bad precision
		- thus truncate to improve precision
- Other
	- [FID Implementation Guide](https://machinelearningmastery.com/how-to-implement-the-frechet-inception-distance-fid-from-scratch/)
	- [FID in PyTorch (GitHub)](https://github.com/bioinf-jku/TTUR)

## 2 - Thu Jan 07 (GAN Disadvantages and Bias)

- Disadvantages
    - lack of intrinsic evaluation metrics
    - unstable training
    - no density estimation
    - inverting is not straightforward
- Advantages
    - amazing empirical results
    - fast inference
- Alternatives to GANs
    - VAEs
        - opposite pros/cons
        - lower fidelity results
        - +density estimation, +inversion, +stable train
    - Autoregressive models
    - Flow models
    - Hybrid models
- Bias
    - disproportionately negative effect on historically undeserved populations
    - Risk assessment software
        - Difficult to validate
        - Misses important considerations about people
- Fairness
    - difficult to define
    - no single definition
    - important to explore before release
    - can be introduced to a model at each step of the process
    - awareness and mitigation of bias is vital to responsible use of AI, and especially SOTA GANs

## 3 - Thu Jan 07 (StyleGAN and Advancements)

- GAN Improvements
    - stability - longer training and better images
        - WGAN-GP, Spectral Normalization
    - capacity - larger models and higher resolution images
    - diversity - increasing variety in generated images
- StyleGAN
    - Goals
        - greater fidelity
        - increased diversity
        - more feature contorol
    - Style: any variation in the image, from large to small (early/late layers)
    - Main components
        - Noise Mapping Network
        - Adaptive Instance Normalization (AdaIN)
        - Progressive Growing
        - Style Mixing
        - Stochastic Noise
- Progressive Growing
    - gradually double image resolution
    - helps with faster and more stable training for higher resolutions
- Noise Mapping Network
    - allows for a more disentangled noise space
    - intermediate noise vector is used as inputs to the generator
- Adaptive Instance Normalization (AdaIN)
    - transfers style information onto the generated image from the intermediate noise vector
    - used to normalize individual examples before applying style statistics from the intermediate noise vector
- Style and Stochastic Variation
    - style mixing increases diversity that the model sees during training
    - stochastic noise causes small variations to output
    - coarse or fineness depends where the network style or noise is added
        - Earlier for coarser variation
        - Later for finer variation
- Resources
    - [Notebook: Freeze D](https://colab.research.google.com/github/https-deeplearning-ai/GANs-Public/blob/master/C2W3_FreezeD_(Optional).ipynb)
    - [StyleGAN 1 & 2](https://jonathan-hui.medium.com/gan-stylegan-stylegan2-479bdf256299)
