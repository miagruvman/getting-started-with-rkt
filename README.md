# Getting started with rkt on Linux

This guide will show you how to install, build and run a "Hello World" application.

### Install rkt
To install rkt we need to download the rkt binary, that can be found on Github.

```
wget https://github.com/coreos/rkt/releases/download/v1.10.0/rkt-v1.10.0.tar.gz
```
and then extract it and go to the rkt-v1.2.0/ directory.
```
tar xzvf rkt-v1.10.0.tar.gz
cd rkt-v1.10.0
```
Check out the rkt commands.
```
./rkt
```

### Install go
To build acbuild on the next step we first need to install go:
```
cd
wget https://storage.googleapis.com/golang/go1.6.linux-amd64.tar.gz
tar xzvf go1.6.linux-amd64.tar.gz
go version
```

### Install acbuild
To install acbuild we will clone the source repository and build it.

```
git clone https://github.com/appc/acbuild
cd acbuild
./build
```

Now we have a bin/ directory. To make sure that your shell can find it executable, we will append this directory to your enviroment´s $PATH variable.

```
vim ~/.bashrc
```
Add the following lines at the end of the file:
```
export ACBUILD_BIN_DIR=~/acbuild/bin
export PATH=$PATH:$ACBUILD_BIN_DIR
```
Set alias to acbuild:
```
alias acbuild="'${PWD}/bin/./acbuild'"
```

### Create the image
Go to ```rkt-v1.10.0/``` directory.

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

You can now see information about your hello application in your pod list:
```
./rkt list
```
