# CUDA not available at build time

Some deep learning libraries refuse to get installed if there is no CUDA-capable
device available. Of course, you can fix this by defining a [default runtime](tips_and_tricks.md#default-runtime)
in your docker configuration. However, this is not an option when your build 
system does not even have a GPU.

Therefore, an alternative is to skip the CUDA device check by inserting the
`FORCE_CUDA` environment variable in your docker build as follows: 

```
ENV FORCE_CUDA="1"
```

# CUDA architectures for PyTorch

If your container needs to build PyTorch (e.g., if you cannot use a PyTorch base image),
then you can supply the list of [NVIDIA architectures](https://docs.nvidia.com/deploy/cuda-compatibility/index.html#support-hardware__table-hardware-support)
via the `TORCH_CUDA_ARCH_LIST` environment variable. By using `ARG`, you can define a 
default value and still override it at build time via the `--build-arg` option:

```
ARG TORCH_CUDA_ARCH_LIST="Kepler;Kepler+Tesla;Maxwell;Maxwell+Tegra;Pascal;Volta;Turing"
ENV TORCH_CUDA_ARCH_LIST="${TORCH_CUDA_ARCH_LIST}"
```


# Interactive timezone prompt

In case you get the following prompt for configuring your timezone data (which will
fail unattended builds):

```
Configuring tzdata
------------------

Please select the geographic area in which you live. Subsequent configuration
questions will narrow this down by presenting a list of cities, representing
the time zones in which they are located.

 1. Africa      4. Australia  7. Atlantic  10. Pacific  13. Etc
 2. America     5. Arctic     8. Europe    11. SystemV
 3. Antarctica  6. Asia       9. Indian    12. US
Geographic area:
```

Then you can precede your command with `DEBIAN_FRONTEND=noninteractive`.

In case of `apt-get`, use something like this:

```commandline
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y ...
```

Or like this:

```commandline
RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get install -y ...
```

Finally, the brute force method is to define it globally via the `ENV` command:

```commandline
ENV DEBIAN_FRONTEND noninteractive
```

# Missing shared library

A common error that you will encounter is installations via `pip` failing due to a shared
library not being present, similar to this:

```
error: XYZ.so: cannot open shared object file: No such file or directory
```

In order to remedy the problem, you need to determine the package that contains this
shared library. In case of Ubuntu (or Debian) you can use the *Ubuntu Packages Search*:

[https://packages.ubuntu.com/](https://packages.ubuntu.com/)

On that page, scroll to the *Search the contents of packages* section and enter
the file name of the missing shared library (e.g., `XYZ.so`). If you know the
distribution (e.g., `bionic` release) and architecture (e.g., `amd64`) then
you can restrict the search further. Finally, click on *Search*. 

From the results page, select the appropriate package name and include that in your 
docker build.


# Repository 'https://developer.download.nvidia.com/compute/cuda/...' is not signed

If installing NVIDIA packages fails (e.g., within Docker) with the following (or similar) error:

```
The repository 'https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64 InRelease' is not signed.
```

This is due to NVIDIA updating/rotating their GPG keys.

Instructions for fixing this error can be found on their developer blog:

[https://developer.nvidia.com/blog/updating-the-cuda-linux-gpg-repository-key/](https://developer.nvidia.com/blog/updating-the-cuda-linux-gpg-repository-key/)

# RuntimeError: cannot cache function '__shear_dense': no locator available for file...

This error occurs when numba has no access to a writable cache directory ([source](https://stackoverflow.com/a/63367171/4698227)). To fix this, make sure that you point the `NUMBA_CACHE_DIR` at a directory with write access, e.g.:

```
ENV NUMBA_CACHE_DIR /tmp
```
