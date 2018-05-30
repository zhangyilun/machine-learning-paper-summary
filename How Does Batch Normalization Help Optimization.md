## How Does Batch Normalization Help Optimization? (No, It Is Not About Internal Covariate Shift)

Link: https://arxiv.org/pdf/1805.11604.pdf
---

At a high level, BatchNorm is a technique that aims to improve training of neural networks by stabilizing the distributions of layer inputs. This is achieved by introducing additional network layers that control the first two moments (mean and variance) of these distributions.

Currently, the most widely accepted explanation of BatchNorm’s success, as well as its original motivation, relates to so-called internal covariate shift (ICS). Informally, ICS refers to the change in the distribution of layer inputs caused by updates to the preceding layers. It is conjectured that such continual change negatively impacts training. The goal of BatchNorm was to reduce ICS and thus remedy this effect.
