<p align="center"><img width="40%" src="docs/source/_static/img/pytorch-logo-dark.png" /></p>

--------------------------------------------------------------------------------
<h2>The point of this fork, is to provide windows support for python 2.7 and 3.5 via conda</h2>
PyTorch is a python package that provides two high-level features:
- Tensor computation (like numpy) with strong GPU acceleration
- Deep Neural Networks built on a tape-based autograd system

You can reuse your favorite python packages such as numpy, scipy and Cython to extend PyTorch when needed.

We are in an early-release Beta. Expect some adventures and rough edges.

- [More About PyTorch](#more-about-pytorch)
- [Installation](#installation)
  - [Binaries](#binaries)
  - [From source](#from-source)
- [Getting Started](#getting-started)
- [Communication](#communication)
- [Releases and Contributing](#releases-and-contributing)
- [The Team](#the-team)

| Python |  **`Linux CPU`**   |  **`Linux GPU`** |
|--------|--------------------|------------------|
| 2.7.8  | [![Build Status](https://travis-ci.com/apaszke/pytorch.svg?token=shqHbUq29zKDxuqzGcjC&branch=master)](https://travis-ci.com/apaszke/pytorch) | |
| 2.7    | [![Build Status](https://travis-ci.com/apaszke/pytorch.svg?token=shqHbUq29zKDxuqzGcjC&branch=master)](https://travis-ci.com/apaszke/pytorch) | [![Build Status](http://build.pytorch.org:8080/buildStatus/icon?job=pytorch-master-py2)](https://build.pytorch.org/job/pytorch-master-py2)  |
| 3.5    | [![Build Status](https://travis-ci.com/apaszke/pytorch.svg?token=shqHbUq29zKDxuqzGcjC&branch=master)](https://travis-ci.com/apaszke/pytorch) | [![Build Status](http://build.pytorch.org:8080/buildStatus/icon?job=pytorch-master-py3)](https://build.pytorch.org/job/pytorch-master-py3)  |
| Nightly| [![Build Status](https://travis-ci.com/apaszke/pytorch.svg?token=shqHbUq29zKDxuqzGcjC&branch=master)](https://travis-ci.com/apaszke/pytorch) | |

## More about PyTorch

At a granular level, PyTorch is a library that consists of the following components:

| \_                       | \_ |
| ------------------------ | --- |
| torch                    | a Tensor library like NumPy, with strong GPU support |
| torch.autograd           | a tape based automatic differentiation library that supports all differentiable Tensor operations in torch |
| torch.nn                 | a neural networks library deeply integrated with autograd designed for maximum flexibility |
| torch.optim              | an optimization package to be used with torch.nn with standard optimization methods such as SGD, RMSProp, LBFGS, Adam etc. |
| torch.multiprocessing    | python multiprocessing, but with magical memory sharing of torch Tensors across processes. Useful for data loading and hogwild training. |
| torch.utils              | DataLoader, Trainer and other utility functions for convenience |
| torch.legacy(.nn/.optim) | legacy code that has been ported over from torch for backward compatibility reasons |

Usually one uses PyTorch either as:

- A replacement for numpy to use the power of GPUs.
- a deep learning research platform that provides maximum flexibility and speed

Elaborating further:

### A GPU-ready Tensor library

If you use numpy, then you have used Tensors (a.k.a ndarray).

<p align=center><img width="30%" src="docs/source/_static/img/tensor_illustration.png" /></p>

PyTorch provides Tensors that can live either on the CPU or the GPU, and accelerate
compute by a huge amount.

We provide a wide variety of tensor routines to accelerate and fit your scientific computation needs
such as slicing, indexing, math operations, linear algebra, reductions.
And they are fast!

### Dynamic Neural Networks: Tape based Autograd

PyTorch has a unique way of building neural networks: using and replaying a tape recorder.

Most frameworks such as `TensorFlow`, `Theano`, `Caffe` and `CNTK` have a static view of the world.
One has to build a neural network, and reuse the same structure again and again.
Changing the way the network behaves means that one has to start from scratch.

With PyTorch, we use a technique called Reverse-mode auto-differentiation, which allows you to
change the way your network behaves arbitrarily with zero lag or overhead. Our inspiration comes
from several research papers on this topic, as well as current and past work such as
[autograd](https://github.com/twitter/torch-autograd),
[autograd](https://github.com/HIPS/autograd),
[Chainer](http://chainer.org), etc.

While this technique is not unique to PyTorch, it's one of the fastest implementations of it to date.
You get the best of speed and flexibility for your crazy research.

<p align=center><img width="80%" src="docs/source/_static/img/dynamic_graph.gif" /></p>

### Python first

PyTorch is not a Python binding into a monolothic C++ framework.
It is built to be deeply integrated into Python.
You can use it naturally like you would use numpy / scipy / scikit-learn etc.
You can write your new neural network layers in Python itself, using your favorite libraries
and use packages such as Cython and Numba.
Our goal is to not reinvent the wheel where appropriate.

### Imperative experiences

PyTorch is designed to be intuitive, linear in thought and easy to use.
When you execute a line of code, it gets executed. There isn't an asynchronous view of the world.
When you drop into a debugger, or receive error messages and stack traces, understanding them is straight-forward.
The stack-trace points to exactly where your code was defined.
We hope you never spend hours debugging your code because of bad stack traces or asynchronous and opaque execution engines.

### Fast and Lean

PyTorch has minimal framework overhead. We integrate acceleration libraries 
such as Intel MKL and NVIDIA (CuDNN, NCCL) to maximize speed. 
At the core, it's CPU and GPU Tensor and Neural Network backends 
(TH, THC, THNN, THCUNN) are written as independent libraries with a C99 API.  
They are mature and have been tested for years.

Hence, PyTorch is quite fast -- whether you run small or large neural networks.

The memory usage in PyTorch is extremely efficient compared to Torch or some of the alternatives.
We've written custom memory allocators for the GPU to make sure that
your deep learning models are maximally memory efficient.
This enables you to train bigger deep learning models than before.

### Extensions without pain

Writing new neural network modules, or interfacing with PyTorch's Tensor API was designed to be straight-forward
and with minimal abstractions.

You can write new neural network layers in Python using the torch API
[or your favorite numpy based libraries such as SciPy](https://github.com/pytorch/tutorials/blob/master/Creating%20extensions%20using%20numpy%20and%20scipy.ipynb).

If you want to write your layers in C/C++, we provide an extension API based on
[cffi](http://cffi.readthedocs.io/en/latest/) that is efficient and with minimal boilerplate.  
There is no wrapper code that needs to be written. [You can see an example here](https://github.com/pytorch/extension-ffi).


## Installation

### Binaries
- Anaconda
```bash
conda install pytorch torchvision -c soumith
```

### From source

Instructions for an Anaconda environment.

If you want to compile with CUDA support, install
- [NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads) 7.5 or above
- [NVIDIA CuDNN](https://developer.nvidia.com/cudnn) v5.x

#### Install optional dependencies

```bash
export CMAKE_PREFIX_PATH=[anaconda root directory]

# Install basic dependencies
conda install numpy mkl setuptools cmake gcc cffi

# On Linux, add LAPACK support for the GPU
conda install -c soumith magma-cuda75 # or magma-cuda80 if CUDA 8.0
```

#### Install PyTorch
```bash
export MACOSX_DEPLOYMENT_TARGET=10.9 # if OSX
pip install -r requirements.txt
python setup.py install
```

## Getting Started

Three pointers to get you started:
- [Tutorials: notebooks to get you started with understanding and using PyTorch](https://github.com/pytorch/tutorials)
- [Examples: easy to understand pytorch code across all domains](https://github.com/pytorch/examples)
- The API Reference: [http://pytorch.org/docs/](http://pytorch.org/docs/)

## Communication
* forums: discuss implementations, research, etc. http://discuss.pytorch.org
* github issues: bug reports, feature requests, install issues, RFCs, thoughts, etc.
* slack: general chat, online discussions, collaboration etc. https://pytorch.slack.com/ . If you need a slack invite, ping us at soumith@pytorch.org
* newsletter: no-noise, one-way email newsletter with important announcements about pytorch. You can sign-up here: http://eepurl.com/cbG0rv

## Releases and Contributing

PyTorch has a 90 day release cycle (major releases). 
It's current state is Beta (v0.1.6), we expect no obvious bugs. Please let us know if you encounter a bug by [filing an issue](https://github.com/pytorch/pytorch/issues).

We appreciate all contributions. If you are planning to contribute back bug-fixes, please do so without any further discussion.

If you plan to contribute new features, utility functions or extensions to the core, please first open an issue and discuss the feature with us.
Sending a PR without discussion might end up resulting in a rejected PR, because we might be taking the core in a different direction than you might be aware of.

**For the next release cycle, these are the 3 big features we are planning to add:**

1. [Distributed PyTorch](https://github.com/pytorch/pytorch/issues/241) (a draft implementation is present in this [branch](https://github.com/apaszke/pytorch-dist) )
2. Backward of Backward - Backpropagating through the optimization process itself. Some past and recent papers such as
   [Double Backprop](http://yann.lecun.com/exdb/publis/pdf/drucker-lecun-91.pdf) and [Unrolled GANs](https://arxiv.org/abs/1611.02163) need this.
3. Lazy Execution Engine for autograd - This will enable us to optionally introduce caching and JIT compilers to optimize autograd code.


## The Team

PyTorch is a community driven project with several skillful engineers and researchers contributing to it.

PyTorch is currently maintained by [Adam Paszke](https://apaszke.github.io/), [Sam Gross](https://github.com/colesbury) and [Soumith Chintala](http://soumith.ch) with major contributions coming from 10s of talented individuals in various forms and means. A non-exhaustive but growing list needs to mention: Sergey Zagoruyko, Adam Lerer, Francisco Massa, Andreas Kopf, James Bradbury, Zeming Lin, Yuandong Tian, Guillaume Lample, Marat Dukhan, Natalia Gimelshein.

Note: this project is unrelated to [hughperkins/pytorch](https://github.com/hughperkins/pytorch) with the same name. Hugh is a valuable contributor in the Torch community and has helped with many things Torch and PyTorch.
