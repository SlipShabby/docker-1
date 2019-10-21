# docker-1
42 SV Docker-1

Docker was created to satisfy this need for unification and normalisation: it makes it
possible to split an application into several microservices, light, adaptable, universal and
scalable, and it also gives the system administrators a great flexibility to deploy and scale
up the app.


docker-1 is a School 42 Docker project.
It consists of three parts:

    How to Docker
    Dockerfiles
    Bonus

At the wiki-pages of project you can find detailed explanations for each task.

docker.en.pdf is the task file.

How to clone?
This repository includes submodule. Submodule is an app for ex02 in 01_dockerfiles part. It is used to demonstrate that created Dockerfile works correctly.

If you want to clone repository with submodule use:

git clone --recurse-submodules <repository url>
If you will use git clone <repository url>, you will get the empty app folder in 01_dockerfiles/ex02.


# Install Homebrew 

1 delete from the homedir .brew </br>
    $ cd ~ </br>
    $ rm -rf .brew </br>

2 install Homebrew with 42 Marvin (message Marvin !brew or !brewbeta)

*:42bounce: Copy and paste the following to install brew :*
```
mkdir $HOME/.brew && curl -fsSL https://github.com/Homebrew/brew/tarball/master | tar xz --strip 1 -C $HOME/.brew
mkdir -p /tmp/.$(whoami)-brew-locks
mkdir -p $HOME/.brew/var/homebrew
ln -s /tmp/.$(whoami)-brew-locks $HOME/.brew/var/homebrew/locks
export PATH="$HOME/.brew/bin:$PATH"
brew update && brew upgrade```
Afterwards make sure you add the following lines to your `.zshrc`.
```mkdir -p /tmp/.$(whoami)-brew-locks
export PATH="$HOME/.brew/bin:$PATH"```
>*Note:*
>Does this look too complicated?
>DM @marvin with `!brewbeta` for an experimental wrapper script.
Click here for more information about brew.


OR

Try out the new Homebrew wrapper script, which should be a lot easier to install.
https://gist.github.com/SuperSpyTX/fd12cc2231b19abc1b82260bdf5802fa

NOTE: you still need to set `$HOME/.brew/bin` in your path, but that should be the only requirement.


All in one go:
```
mkdir -p $HOME/.brew/bin;
curl https://gist.githubusercontent.com/SuperSpyTX/fd12cc2231b19abc1b82260bdf5802fa/raw/5ee82acffd9e57e94e1a6ad0c86f0f3802e9f9fc/brew-42 > $HOME/.brew/bin/brew-42;
chmod +x $HOME/.brew/bin/brew-42;
grep 'PATH=$HOME/.brew/bin' ~/.zshrc 2>/dev/null 1>&2 || echo 'export PATH=$HOME/.brew/bin:$PATH;' >> ~/.zshrc;
source ~/.zshrc;
brew-42 update;
```

Note that this is beta, there might be permission errors for /usr/local.

# Install Docker

$ brew install docker docker-machine
$ docker --version

Good article to read: 
https://medium.com/@yutafujii_59175/a-complete-one-by-one-guide-to-install-docker-on-your-mac-os-using-homebrew-e818eb4cfc3



Best practice to create Dockerfile:
https://docs.docker.com/develop/develop-images/dockerfile_best-practices/