### How Does Batch Normalization Help Optimization? (No, It Is Not About Internal Covariate Shift)

####Link: https://arxiv.org/pdf/1805.11604.pdf
---

At a high level, **BatchNorm** is a technique that aims to improve training of neural networks by stabilizing the distributions of layer inputs. This is achieved by introducing additional network layers that control the first two moments (mean and variance) of these distributions. In keras, `BatchNormalization` maintains the mean activation close to 0 and the activation standard deviation close to 1.

Currently, the most widely accepted explanation of BatchNorm’s success, as well as its original motivation, relates to so-called **internal covariate shift (ICS)**. Informally, ICS refers to the change in the distribution of layer inputs caused by updates to the preceding (previous) layers. It is conjectured that such continual change negatively impacts training. The goal of BatchNorm was to reduce ICS and thus remedy this effect.

The paper demonstrates that BatchNorm impacts network training in a fundamental way: **it makes the landscape of the corresponding optimization problem be significantly more smooth**. This ensures, in particular, that the gradients are more predictive and thus allow for use of larger range of learning rates and faster network convergence. It is also found that this smoothening effect is not uniquely tied to BatchNorm. A number of other natural normalization techniques has a similar (and, sometime, even stronger) effect. In particular, they all offer similar improvements in the training performance.

#### Experiment 1: VGG with CIFAR-10, BatchNorm vs. no BatchNorm

Relatively clear gap on performance (accuracy) between two models, but much less pronounced difference between layer input distributions. Further adding a model with BatchNorm and noise injuection after BatchNorm layers, the paper discovered that the noisy BatchNorm network has *less stable distrubtions* than the non-BatchNorm network, but *still performs better* in terms of training.

