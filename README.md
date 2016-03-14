# Getting started with rkt on Linux

This guide will show you how to install, build and run a "Hello World" application.

### Install rkt

To install rkt we need to download the rkt binary, that can be found on Github.

```
wget https://github.com/coreos/rkt/releases/download/v1.1.0/rkt-v1.1.0.tar.gz
```
and then extract it and go to the rkt-v1.1.0/ directory.
```
tar xzvf rkt-v1.1.0.tar.gz
cd rkt-v1.1.0
```
Check out the rkt commands.
```
./rkt
```

### Install acbuild

To install acbuild we will clone the source repository.

```
git clone https://github.com/appc/acbuild
```
```
cd acbuild
```
```
./build
```

To make sure that your shell can find the bin/ directory executable, we will append this directory to your enviroment´s $PATH variable.

```
vim ~/.bashrc
```
... and put the following lines at the end of the file:
```
export ACBUILD_BIN_DIR=~/acbuild/bin
export PATH=$PATH:$ACBUILD_BIN_DIR
```

### Create the image

Now we can use acbuild to create the image.

```
acbuild --debug begin
acbuild --debug set-name hello
acbuild --debug dep add quay.io/coreos/alpine-sh
acbuild --debug set-exec -- /bin/echo Hello World!
acbuild --debug write hello.aci
acbuild --debug end
```

### Run
To launch our application image:
```
sudo ./rkt --insecure-options=image run hello.aci
```

You can now see your hello application in your local image list:
```
./rkt image list
```
