---
layout: post
title:  "Setting up Kaggle Python Docker on Linux in 2018"
date:   2018-07-08
categories: coding kaggle
tags: python docker kaggle
---

Here is a more up-to-date version of how to set up Kaggle/Python on Ubuntu 18.04 (Bionic).

# Install Docker-CE

You can use the [installation script from Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) directly.

```bash
# Remove older versions
sudo apt-get remove docker docker-engine docker.io
curl -fsSL get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

# Get Kaggle/Python

According to the [offical GitHub page](https://github.com/Kaggle/docker-python), you can fetch the image by running the following command:

```bash
docker run --rm -it kaggle/python
```

If you happen to have a permission error that looks like

```
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock
```

Try check if your user is in the `docker` group using the `groups` command.

```bash
groups $USER                     
dash : dash adm cdrom sudo dip plugdev lpadmin sambashare docker
```
If you do not see a `docker` group, run the follow command:

```bash
sudo usermod -a -G docker dash
```

and check `groups $USER` again. After this, you may need to restart your machine to take effect.

# Set up command short-cuts

Put the following scripts into either `~/.bashrc` or `~/.zshrc`. Fish-shell would be a littble different, but I no longer using it now :(

```bash
# Kaggle Python Docker Commands

kpython() {
    docker run -v $PWD:/tmp/working -w=/tmp/working --rm -it kaggle/python python "$@"
}
kpython2() {
    docker run -v $PWD:/tmp/working -w=/tmp/working --rm -it kaggle/python python2 "$@"
}
kpython3() {
    docker run -v $PWD:/tmp/working -w=/tmp/working --rm -it kaggle/python python3 "$@"
}
kipython() {
    docker run -v $PWD:/tmp/working -w=/tmp/working --rm -it kaggle/python ipython
}
kipython2() {
    echo "NO ipython with python2 available."
}
kipython3() {
    docker run -v $PWD:/tmp/working -w=/tmp/working --rm -it kaggle/python ipython
}

kjupyter() {
    docker run -v $PWD:/tmp/working -w=/tmp/working -p 8888:8888 --rm -it kaggle/python jupyter notebook --no-browser --ip="0.0.0.0" --notebook-dir=/tmp/working --allow-root
}

kjupyterlab() {
    docker run -v $PWD:/tmp/working -w=/tmp/working -p8808:8808 --rm -it kaggle/python jupyter lab --no-browser --ip="0.0.0.0" --port=8808 --notebook-dir=/tmp/working --allow-root
}
```

Then open a new terminal, and test out the following commands:

* `kpython` and `kpython3`: start `python3`
* `kpython2`: start `python2`
* `kipython` and `kipython`: start `ipython` using `python3` as the kernel
* `kjupyter`: start Jupyter notebook at `0.0.0.0:8888`
* `kjupyterlab`: start Jupyter lab at `0.0.0.0:8808`

Enjoy, and let me know what you thinks.
