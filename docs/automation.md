Building docker images by hand is fine when you are developing a new image. However, once
the development phase is over and maintenance starts, you will only make minor changes.
Having to build and push out an image then becomes tedious. Instead, automate the build
and push process using a build system.

Of course, there are a million and one different build tools out there and you will have
to find the one that you like best and that fits your environment.

Coming from a Java/Maven background, I was already familiar with 
[Jenkins](https://www.jenkins.io/). It may not be the best of interfaces, but if you 
are only occasionally performing builds then it is nice if you can just parametrize 
some plugins through a web interface and concentrate on other tasks. For my docker builds, 
I use the [Docker plugin for Jenkins](https://plugins.jenkins.io/docker-plugin/). For publicly
images hosted on public registries that use public git repositories, the plugin
is really easy to use. Using Jenkins' credential mechanism, you can also push out
images to private registries (for cloning/pulling from private git repositories, you 
can employ some [ssh-config magic](http://open.fracpete.org/2017/09/jenkins-and-private-repos-on-github/)). 

Other (untested) approaches you could look into are:

* [gradle](https://tomgregory.com/automating-docker-builds-with-gradle/)
* [github actions](https://github.com/marketplace/actions/build-and-push-docker-images)
