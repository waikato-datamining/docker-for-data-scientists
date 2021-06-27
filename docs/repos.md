A lot of open-source deep learning projects will supply docker base images already
on [docker hub](https://hub.docker.com/), which will make your life much easier.
By using these base images, you will only need to install additional libraries that
your own project requires.

Here is a (non-exhaustive) list of repositories on docker hub: 

* [Ubuntu](https://hub.docker.com/_/ubuntu)
* [CUDA](https://hub.docker.com/r/nvidia/cuda)
* [PyTorch](https://hub.docker.com/r/pytorch/pytorch) (tend to use Anaconda, unfortunately)
* [Tensorflow](https://hub.docker.com/r/tensorflow/tensorflow)

Of course, NVIDIA likes to keep things close to its heart and offers their own
registry from which you can also retrieve images for the various IoT devices that
NVIDIA has on offer:

* [NGC catalog](https://ngc.nvidia.com/catalog/containers)
* [NVIDIA L4T Base](https://ngc.nvidia.com/catalog/containers/nvidia:l4t-base)  
* [NVIDIA L4T PyTorch](https://ngc.nvidia.com/catalog/containers/nvidia:l4t-pytorch)
* [NVIDIA L4T Tensorflow](https://ngc.nvidia.com/catalog/containers/nvidia:l4t-tensorflow)
