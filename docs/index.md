From one data scientist to another on how to utilize [docker](https://www.docker.com/) 
to make your life easier.

![Docker](img/docker-logo.png)

Any data scientist that ever had to set up an environment for a deep learning
framework, knows that getting the combination of [CUDA](https://developer.nvidia.com/cuda), 
[cuDNN](https://developer.nvidia.com/cudnn), deep learning framework and other 
libraries right is a frustrating exercise. With docker you still have to go through 
the pain of figuring out the right combination, but... And it is a **BIG** but! Once you 
have this blueprint called a *docker image*, you can use it on other machines as well; you 
will be up and running in seconds.

This is by no means supposed to be an exhaustive introduction to docker (docker 
can do a lot more!), but merely for getting you started on your journey.

**Disclaimer:** This introduction was written using a Linux host system. If you are using
Mac OSX or WSL under Windows, your mileage may vary...

# Sections

To get started with docker, you should look at the following sections:

* [Basics](basics.md) - explains the most common operations that you will need to know 
* [Dockerfile](dockerfile.md) - explains the structure of the `Dockerfile` file which 
  is used to create docker images
* [Best practices](best_practices.md) - what to keep in mind when creating and using images
* [Repositories](repos.md) - where to find the base images that your own images will use

If you should encounter problems, then have a look here:

* [Troubleshooting](troubleshooting.md)

Just like with any programming language or complex framework, there are certain
things that can make your life easier. Therefore do not forget to have a look at:

* [Tips & Tricks](tips_and_tricks.md)

Once you get a handle on things, and you are getting tired of manually building images, 
you might want to look into automating your builds and maybe also run your own 
registry/proxy. In that case, have a look at the following sections:

* [Automation](automation.md)
* [Registry](registry.md)

Finally, if you need to orchestrate multiple docker images, you can have a look at:

* [docker compose](https://docs.docker.com/compose/)


# About the content

This page was generated using [mkdocs](https://www.mkdocs.org/). The source code itself is hosted at [github.com/waikato-datamining/docker-for-data-scientists](https://github.com/waikato-datamining/docker-for-data-scientists) and licensed under [CC-BY-SA 4.0](https://github.com/waikato-datamining/docker-for-data-scientists/blob/main/LICENSE).
