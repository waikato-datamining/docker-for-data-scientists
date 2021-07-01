# Versioning

Since docker determines by the hash of a command whether this particular layer needs
rebuilding, you should, wherever possible, specify the version of the library that
you are installing. That way, whenever you upgrade a library, that layer (and all 
subsequent ones) will get rebuilt. The added bonus is that you will not accidentally 
rebuild the image with a newer version of a library at a later stage that may now be 
incompatible with all the others (*Yeah, I'm looking at you, numpy*!).

So instead of:

```commandline
RUN python3 -m pip install --no-cache-dir numpy && \
    ... 
```

You should do something like this:

```commandline
RUN python3 -m pip install --no-cache-dir numpy==1.17.4 && \
    ... 
```

Of course, when you are cloning directly from a github repository, because you require
a specific bugfix or the library does not offer any releases (or only very infrequent),
then you should use a specific commit hash in your command:  

```commandline
RUN git clone https://github.com/ACCOUNT/REPO.git && \
    cd REPO && \
    git reset --hard 11223344556677889900AABBCCDDEEFF11223344 && \
    ...
```


# Docker group

Instead of running the `docker` command via `sudo`, you should consider adding your user
to the `docker` group instead (in `/etc/group`). That way, you can run the `docker` 
command as a regular user.


# Launch container as regular user

Once development of your docker image has finished, you should avoid running docker 
containers as the `root` user and instead run it as the current user (which also 
avoids creating output files in volumes that can only be removed by `root`).

Use the `-u` and `-e` command to specify the user/group IDs and the user name 
as follows:

```
docker run -u $(id -u):$(id -g) -e USER=$USER ...
```

However, the environment variables for you command prompt (when you are using your
container in interactive mode) may not be able to handle this properly. In such a
case you will get output similar to this: 

```
groups: cannot find name for group ID XYZ
I have no name!@c7355d9a1b59:/$ 
```

You can rectify this by creating a custom `bash.bashrc`file using the 
[docker-banner-gen](https://github.com/waikato-datamining/docker-banner-gen) library
(which not only outputs a nice banner, but also warns you in case you are running
the container as `root`).

This custom file can then be added to your docker image using the following command:

```commandline
COPY bash.bashrc /etc/bash.bashrc
```


# No Anaconda

Instead of using [Anaconda](https://www.anaconda.com/products/individual) for installing
Python packages, you can just use plain `pip` to install packages from the [Python Package Index](https://pypi.org/).
Anaconda in itself is quite a large installation and will increase the overall docker
image size unnecessarily.


# Clean up pip

Remove the pip cache to reduce the size of your layer after you have installed all your packages:

```
rm -Rf /root/.cache/pip
```

Alternatively, do not cache downloads when installing a package:

```
pip install --no-cache-dir ...
```


# Clean up apt

After performing installs via `apt` or `apt-get`, clean up the cache to reduce the 
size of your layer:

```
apt-get clean && \
rm -rf /var/lib/apt/lists/*
```

# Caching models

As soon as you stop/remove the container, all modifications will be lost. This also includes
any pretrained networks that any of the deep learning frameworks downloaded into its cache.
In order to avoid the constant downloads, you can map the cache directories for the relevant
framework to a local directory on the host:

**PyTorch**

* when running the container as `root`:

    ```
    -v /some/where/cache:/root/.torch \
    ```

* when running the container as regular user:

    ```
    -v /some/where/cache:/.torch \
    ```
    
**Keras**

* Map the cache directory (`/root/.keras`) when running container as `root`:

    ```
    -v /some/where/cache:/root/.keras
    ```

* Or when running as regular user (`/tmp/.keras`):

    ```
    -v /some/where/cache:/tmp/.keras
    ```
