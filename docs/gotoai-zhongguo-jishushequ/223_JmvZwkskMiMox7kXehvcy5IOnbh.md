# Advanced-Deep-Learning-with-Keras

> 来源：[https://gotoaiworld.feishu.cn/docx/QnEXdKwwzo0N3lxJr0ecczU5nKh](https://gotoaiworld.feishu.cn/docx/QnEXdKwwzo0N3lxJr0ecczU5nKh)

# Advanced-Deep-Learning-with-Keras

https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras

![](img/d8d052317b54c1dfda232570c1c0ebb2.png)

# Advanced Deep Learning with TensorFlow 2 and Keras (Updated for 2nd Edition)

[This is the code repository fo](https://www.packtpub.com/programming/advanced-deep-learning-with-tensorflow-2-and-keras-second-edition)r Advanced Deep[ Lear](https://www.packtpub.com/?utm_source=github)ning with TensorFlow 2 and Keras, published by Packt. It contains all the supporting project files necessary to work through the book from start to finish.

Please note that the code examples have been updated to support TensorFlow 2.0 Keras API only.

## About the Book

Advanced Deep Learning with TensorFlow 2 and Keras, Second Edition is a completely updated edition of the bestselling guide to the advanced deep learning techniques available today. Revised for TensorFlow 2.x, this edition introduces you to the practical side of deep learning with new chapters on unsupervised learning using mutual information, object detection (SSD), and semantic segmentation (FCN and PSPNet), further allowing you to create your own cutting-edge AI projects.

Using Keras as an open-source deep learning library, the book features hands-on projects that show you how to create more effective AI with the most up-to-date techniques.

Starting with an overview of multi-layer perceptrons (MLPs), convolutional neural networks (CNNs), and recurrent neural networks (RNNs), the book then introduces more cutting-edge techniques as you explore deep neural network architectures, including ResNet and DenseNet, and how to create autoencoders. You will then learn about GANs, and how they can unlock new levels of AI performance.

Next, you’ll discover how a variational autoencoder (VAE) is implemented, and how GANs and VAEs have the generative power to synthesize data that can be extremely convincing to humans. You'll also learn to implement DRL such as Deep Q-Learning and Policy Gradient Methods, which are critical to many modern results in AI.

## Related Products

+   [Deep Reinforceme](https://www.packtpub.com/big-data-and-business-intelligence/deep-reinforcement-learning-hands?utm_source=github&utm_medium=repository&utm_campaign=9781788834247)nt Learning Hands-On

+   Deep Learning with Keras

+   [Reinforcement Lear](https://www.packtpub.com/big-data-and-business-intelligence/reinforcement-learning-tensorflow?utm_source=github&utm_medium=repository&utm_campaign=9781788835725)ning with TensorFlow

## Installation

It is recommended to run within conda [environm](https://www.anaconda.com/distribution)ent. Pls download Anacoda from: Anaconda. To install anaconda:

sh <name-of-downloaded-Anaconda3-installer>

A machine with at least 1 NVIDIA GPU (1060 or better) is required. The code examples have been tested on 1060, 1080Ti, RTX 2080Ti, V100, RTX Quadro 8000 on Ubuntu 18.04 LTS. Below is a rough guide to install NVIDIA driver and CuDNN to enable GPU support.

sudo add-apt-repository ppa:graphics-drivers/ppa

sudo apt update

sudo ubuntu-drivers autoinstall

sudo reboot

nvidia-smi

At the time of writing, nvidia-smishows the NVIDIA driver version is 440.64 and CUDA version is 10.2.

We are almost there. The last set of packages must be installed as follows. Some steps might require sudo access.

conda create --name packt

conda activate packt

cd <github-dir>

git clone https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras

cd Advanced-Deep-Learning-with-Keras

pip install -r requirements.txt

sudo apt-get install python-pydot

sudo apt-get install ffmpeg

Test if a simple model can be trained without errors:

cd chapter1-keras-quick-tour

python3 mlp-mnist-1.3.2.py

The final output shows the accuracy of the trained model on MNIST test dataset is about 98.2%.

### Alternative TensorFlow Installation

If you are having problems with CUDA libraries (ie tf could not load or find libcudart.so.10.X), TensorFlow and CUDA libraries can be installed together using conda:

```
pip uninstall tensorflow-gpu
conda install -c anaconda tensorflow-gpu
```

# [Advanced Deep Learning with Te](https://amzn.to/2wotTnN)nsorFlow 2 and Keras code examples used in the book.

## Chapter 1 - Introduction

1.  [MLP on MNIST](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter1-keras-quick-tour/mlp-mnist-1.3.2.py)

1.  [CNN on MNIST](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter1-keras-quick-tour/cnn-mnist-1.4.1.py)

1.  [RNN on MNIST](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter1-keras-quick-tour/rnn-mnist-1.5.1.py)

## Chapter 2 - Deep Networks

1.  Functional API on MNIST

1.  Y-Network on MNIST

1.  ResNet v1 and v2 on CIFAR10

1.  DenseNet on CIFAR10

## Chapter 3 - AutoEncoders

1.  Denoising AutoEncoders

Sample outputs for random digits:

![](img/555c28a9ffb8a5aeebf48baa8a75ad8e.png)

1.  Colorization AutoEncoder

Sample outputs for random cifar10 images:

## [Chapter 4 - Generative Adver](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter4-gan)sarial Network (GAN)

1.  Deep Convolutional GAN (DCGAN)

[Radford, Alec, Luke Metz, and Soumith Chintala. "Unsupervised representation learni](https://arxiv.org/pdf/1511.06434.pdf%C3%AF%C2%BC%E2%80%B0)ng with deep convolutional generative adversarial networks." arXiv preprint arXiv:1511.06434 (2015).

Sample outputs for random digits:

1.  Conditional (GAN)

[Mirza, Mehdi, and Simon Osindero. "Conditional generati](https://arxiv.org/pdf/1411.1784)ve adversarial nets." arXiv preprint arXiv:1411.1784 (2014).

Sample outputs for digits 0 to 9:

## Chapter 5 - Improved GAN

1.  Wasserstein GAN (WGAN)

[Arjovsky, Martin, Soumith Chintala, and Léon Bottou](https://arxiv.org/pdf/1701.07875). "Wasserstein GAN." arXiv preprint arXiv:1701.07875 (2017).

Sample outputs for random digits:

1.  Least Squares GAN (LSGAN)

[Mao](http://openaccess.thecvf.com/content_ICCV_2017/papers/Mao_Least_Squares_Generative_ICCV_2017_paper.pdf), Xudong, et al. "Least squares generative adversarial networks." 2017 IEEE International Conference on Computer Vision (ICCV). IEEE, 2017.

Sample outputs for random digits:

1.  Auxiliary Classifier GAN (ACGAN)

[Odena, Augustus, Christopher Olah, and Jonathon Shlens. "Conditional image synthesis with auxiliary c](http://proceedings.mlr.press/v70/odena17a.html)lassifier GANs. Proceedings of the 34th International Conference on Machine Learning, Sydney, Australia, PMLR 70, 2017."

Sample outputs for digits 0 to 9:

## [C](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter6-disentangled-gan)hapter 6 - GAN with Disentangled Latent Representations

1.  [Information Maxi](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter6-disentangled-gan/infogan-mnist-6.1.1.py)mizing GAN (InfoGAN)

[Chen](http://papers.nips.cc/paper/6399-infogan-interpretable-representation-learning-by-information-maximizing-generative-adversarial-nets.pdf), Xi, et al. "Infogan: Interpretable representation learning by information maximizing generative adversarial nets." Advances in Neural Information Processing Systems. 2016.

Sample outputs for digits 0 to 9:

![](img/77c124e6937ec0a8cfe64f634a9eac1a.png)

1.  [Stacked GAN](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter6-disentangled-gan/stackedgan-mnist-6.2.1.py)

[Hua](http://openaccess.thecvf.com/content_cvpr_2017/papers/Huang_Stacked_Generative_Adversarial_CVPR_2017_paper.pdf)ng, Xun, et al. "Stacked generative adversarial networks." IEEE Conference on Computer Vision and Pattern Recognition (CVPR). Vol. 2. 2017

Sample outputs for digits 0 to 9:

![](img/1f942722e282c3193e106aa4de93fac0.png)

## Chapter 7 - Cross-Domain GAN

1.  [CycleGAN](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter7-cross-domain-gan/cyclegan-7.1.1.py)

[Zhu,](http://openaccess.thecvf.com/content_ICCV_2017/papers/Zhu_Unpaired_Image-To-Image_Translation_ICCV_2017_paper.pdf) Jun-Yan, et al. "Unpaired Image-to-Image Translation Using Cycle-Consistent Adversarial Networks." 2017 IEEE International Conference on Computer Vision (ICCV). IEEE, 2017.

Sample outputs for random cifar10 images:

Sample outputs for MNIST to SVHN:

![](img/1c6aa7bd702215077834d5e5a2d613a3.png)

## [Chapter 8 - Variationa](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter8-vae)l Autoencoders (VAE)

1.  [VAE MLP MNIST](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter8-vae/vae-mlp-mnist-8.1.1.py)

1.  [VAE CNN MNIST](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter8-vae/cvae-cnn-mnist-8.2.1.py)

1.  Conditional VAE and Beta VAE

[Kingma, Diederik P., and Max Welling. "Auto-encodin](https://arxiv.org/pdf/1312.6114.pdf)g Variational Bayes." arXiv preprint arXiv:1312.6114 (2013).

[Sohn, Kihyuk, Honglak Lee, and Xinchen Yan. "Learning structured output representation](http://papers.nips.cc/paper/5775-learning-structured-output-representation-using-deep-conditional-generative-models.pdf) using deep conditional generative models." Advances in Neural Information Processing Systems. 2015.

[I. Higgins, L. Matthey, A. Pal, C. Burgess, X. Glorot, M. Botvinick, S. Mohamed, and A. L](https://openreview.net/pdf?id=Sy2fzU9gl)erchner. β-VAE: Learning basic visual concepts with a constrained variational framework. ICLR, 2017.

Generated MNIST by navigating the latent space:

## [Chapter 9 - Deep Re](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter9-drl)inforcement Learning

1.  [Q-Learning](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter9-drl/q-learning-9.3.1.py)

1.  [Q-Learning on Fro](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter9-drl/q-frozenlake-9.5.1.py)zen Lake Environment

1.  [DQN and DDQN on ](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter9-drl/dqn-cartpole-9.6.1.py)Cartpole Environment

Mnih, Volodymyr, et al. "Human-level control through deep reinforcement learning." Nature 518.7540 (2015): 529

DQN on Cartpole Environment:

## [Chapter 10 - Pol](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter10-policy)icy Gradient Methods

1.  [R](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter10-policy/policygradient-car-10.1.1.py)EINFORCE, REINFORCE with Baseline, Actor-Critic, A2C

[S](http://incompleteideas.net/book/bookdraft2017nov5.pdf)utton and Barto, Reinforcement Learning: An Introduction

[Mni](http://proceedings.mlr.press/v48/mniha16.pdf)h, Volodymyr, et al. "Asynchronous methods for deep reinforcement learning." International conference on machine learning. 2016.

Policy Gradient on MountainCar Continuous Environment:

## Chapter 11 - Object Detection

1.  Single-Shot Detection

Single-Shot Detection on 3 Objects

## Chapter 12 - Semantic Segmentation

1.  [FCN](https://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Long_Fully_Convolutional_Networks_2015_CVPR_paper.pdf)

1.  [PSPNet](http://openaccess.thecvf.com/content_cvpr_2017/papers/Zhao_Pyramid_Scene_Parsing_CVPR_2017_paper.pdf)

Semantic Segmentation

## [C](https://github.com/PacktPublishing/Advanced-Deep-Learning-with-Keras/blob/master/chapter13-mi-unsupervised)hapter 13 - Unsupervised Learning using Mutual Information

1.  Invariant Information Clustering

1.  MINE: Mutual Information Estimation

MINE

![](img/e5ff0511d26106f645fc8050492b69a0.png)

## Citation

If you find this work useful, please cite:

```
@book{atienza2020advanced,
  title={Advanced Deep Learning with TensorFlow 2 and Keras: Apply DL, GANs, VAEs, deep RL, unsupervised learning, object detection and segmentation, and more},
  author={Atienza, Rowel},
  year={2020},
  publisher={Packt Publishing Ltd}
}
```