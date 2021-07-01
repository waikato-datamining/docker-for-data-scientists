# Remove unwanted files

Especially during development, it can happen that files get generated when running
your docker container that can only be removed by the `root` user again. If you 
do not have admin permissions (e.g., via `sudo`) on the machine then you can 
revert to using docker to remove them again:

* change into the directory with unwanted files and/or directories

* the following docker command will map the current directory to `/opt/local`:

    ```
    docker run -v `pwd`:/opt/local -it bash:5.1.8
    ```
  
* change into `/opt/local` and remove all files/dirs (or just the files that need removing):

    ```
    cd /opt/local
    rm -Rf *
    ```

# Alternative Python version

In case your base Debian/Ubuntu distribution does not have the right Python version 
available, you can get additional versions by adding the [deadsnakes ppa](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa):

```commandline
add-apt-repository -y ppa:deadsnakes/ppa && \
apt-get update
```

# Switch default Python version

If your base distro has an older version of Python and you do not want to sprinkle the
specific newer version throughout your `Dockerfile`, then you can use `update-alternatives`
on Debian systems to change the default.

The following commands switch from Python 3.5 to Python 3.6 for the `python3` executable
and also use Python 3.6 as the default for the `python` executable:

```commandline
update-alternatives --install /usr/bin/python python /usr/bin/python3.5 1 && \
update-alternatives --install /usr/bin/python python /usr/bin/python3.6 2 && \
update-alternatives --set python /usr/bin/python3.6 && \
update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 1 && \
update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2 && \
update-alternatives --set python3 /usr/bin/python3.6 && \
```


# Screen

The [screen](https://linux.die.net/man/1/screen) command-line utility allows you to 
pick up remote sessions after detaching from them. This is especially helpful when 
running docker in interactive mode via ssh sessions. If such an ssh session should
accidentally close (e.g., internet connection lost, laptop closed), then the
docker command would terminate as well.

On the remote host, **start** the `screen` command before you launch your docker command 
(you can run multiple `screen` instances as well) and skip the screen with the licenses 
etc using `Enter` or `Space`. Now you can start up the actual command. If you want to 
run multiple screen sessions on the same host, then you should name them using the 
`-S sessionname` option to easily distinguish them.

For **detaching** a session, use the `CTRL+A+D` key combination. This will leave your 
process running in the background and you can close the remote connection.

In order to **reconnect**, simply ssh into the remote host and run `screen -r`. If there
is only one screen session running, then it will automatically reconnect. Otherwise, it
will output a list of available sessions. Supply the name of the session that you want to 
reattach to the `-r` option.

You can **exit** a screen session by either typing `exit` or using `CTRL+D` (just like
with a `bash` shell).

If you need to increase the scrollback buffer, let us say to 100,000 lines, then you can do it

* in the current session as follows:

    ```
    CTRL A : <Enter>
    scrollback 100000<Enter>
    ```

* for all new sessions by adding the following line in your `$HOME/.screenrc` config file:

    ```
    defscrollback 100000
    ```


# Default runtime

Instead of always having to type `--runtime=nvidia` or `--gpus=all`, you can simply define
the *default runtime* to use with your docker commands ([source](https://docs.nvidia.com/dgx/nvidia-container-runtime-upgrade/index.html)):

* edit the `/etc/docker/daemon.json` file as `root` user
* insert the key `default-runtime` with value `nvidia` so that it looks something like this:
  
```json
{
    "default-runtime": "nvidia",
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}
```

* save the file
* restart the docker daemon (e.g., `sudo systemctl restart docker`)


# Thorough clean up

You can stop/remove all containers and images with this one-liner:

```commandline
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q) && docker system prune -a
```

If you need to remove dangling volumes, use this:

```commandline
docker volume prune
```
