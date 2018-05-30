### How Does Batch Normalization Help Optimization? (No, It Is Not About Internal Covariate Shift)
---

#### Link: https://arxiv.org/pdf/1805.11604.pdf

At a high level, **BatchNorm** is a technique that aims to improve training of neural networks by stabilizing the distributions of layer inputs. This is achieved by introducing additional network layers that control the first two moments (mean and variance) of these distributions. In keras, `BatchNormalization` maintains the mean activation close to 0 and the activation standard deviation close to 1.

Currently, the most widely accepted explanation of BatchNorm’s success, as well as its original motivation, relates to so-called **internal covariate shift (ICS)**. Informally, ICS refers to the change in the distribution of layer inputs caused by updates to the preceding (previous) layers. It is conjectured that such continual change negatively impacts training. The goal of BatchNorm was to reduce ICS and thus remedy this effect.

The paper demonstrates that BatchNorm impacts network training in a fundamental way: **it makes the landscape of the corresponding optimization problem be significantly more smooth**. This ensures, in particular, that the gradients are more predictive and thus allow for use of larger range of learning rates and faster network convergence. It is also found that this smoothening effect is not uniquely tied to BatchNorm. A number of other natural normalization techniques has a similar (and, sometime, even stronger) effect. In particular, they all offer similar improvements in the training performance.

**Experiment 1: VGG with CIFAR-10, BatchNorm vs. no BatchNorm.** Relatively clear gap on performance (accuracy) between two models, but much less pronounced difference between layer input distributions. Further adding a model with BatchNorm and noise injuection after BatchNorm layers, the paper discovered that the noisy BatchNorm network has *less stable distrubtions* than the non-BatchNorm network, but *still performs better* in terms of training.

**Experiment 2: Measurement of ICS in networks with and without BatchNorm layers.** Tested two networks: VGG and a DLN with 25 layers. It is shown that models with BatchNorm have similar, or even worse, internal covariate shift, despite performing better in terms of accuracy and loss.

**Why BatchNorm works.** The smoothing effect of BatchNorm. By looking at the training/validation loss landscape, models with BatchNorm has much smaller loss variation (much less fluctuation) when compared to the standard model without BatchNorm. This can affect the training significantly since the loss function that is being optimized is usually non-convex and tens to have a large number of "kinks", flat regions and sharp minima. This makes training gradient descent-based algorithm very unstable. BatchNorm is making the gradients more reliable and predictive (Lipschitzness of gradients of the loss). It will also allow larger learning rates and less hassle in hyperparameter choices (less sensitive).

**Other Normalization.** $l_p$ norms are tested along with BatchNorm and comparable performance are observed compared to BatchNorm. Note that $l_p$ norms lead to larger distributional covariate shift, but still yielding improved optimization performance. All $l_p$ normalization techniques result in an improved smoothness of the landscape that is similar to the effect of BatchNorm.
