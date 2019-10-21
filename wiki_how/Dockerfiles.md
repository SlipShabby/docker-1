### Exercise 00: vim/emacs

Exercise 00: vim/emacs
:-----
Turn-in directory : ex00/
Files to turn in : Dockerfile
Allowed functions : -
Notes : n/a

From an `alpine` image you’ll add to your container your favorite text editor, `vim` or `emacs`, that will launch along with your container.

**Answer**



```
FROM alpine

MAINTAINER vbrazhni <vbrazhni@student.unit.ua>

RUN apk update && \
    apk upgrade && \
    apk add emacs

ENTRYPOINT emacs

# docker-machine restart Char
# eval $(docker-machine env Char)

# How to build it?
# docker build -t ex00 .

# How to run it?
# docker run --rm -ti ex00

# docker images ls

```



### Exercise 01: BYOTSS

Exercise 01: BYOTSS
:-----
Turn-in directory : ex01/
Files to turn in : Dockerfile + Scripts (if applicable)
Allowed functions : -
Notes : n/a

From a debian image you will add the appropriate sources to create a TeamSpeak server, that will launch along with your container. It will be deemed valid if at least one user can connect to it and engage in a normal discussion (no far-fetched setup), so be sure to create your Dockerfile with the right options. Your program should get the sources when it builds, they cannot be in your repository.

> For the smarty-pants using the official docker image of TeamSpeak is strictly FORBIDDEN, and will get you -42.

**Answer**

```
FROM debian

MAINTAINER vbrazhni <vbrazhni@student.unit.ua>

ENV TS3SERVER_LICENSE=accept

WORKDIR /home/teamspeak

EXPOSE 9987/udp 10011 30033

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y wget bzip2 && \
    wget http://dl.4players.de/ts/releases/3.0.13.4/teamspeak3-server_linux_amd64-3.0.13.4.tar.bz2 && \
    tar -xvf teamspeak3-server_linux_amd64-3.0.13.4.tar.bz2

WORKDIR teamspeak3-server_linux_amd64

ENTRYPOINT sh ts3server_minimal_runscript.sh

# Writing EXPOSE in your Dockerfile, is merely a hint that a certain port is useful. 
# Docker won’t do anything with that information by itself.

# How to build it?
# docker build -t ex01 .

# How to run it?
# docker run --rm -p 9987:9987/udp -p 10011:10011 -p 30033:30033 ex01
```

### Exercise 02: Dockerfile in a Dockerfile... in a Dockerfile ?

Exercise 02: Dockerfile in a Dockerfile... in a Dockerfile ?
:-----
Turn-in directory : ex02/
Files to turn in : Dockerfile
Allowed functions : -
Notes : n/a

You are going to create your first Dockerfile to containerize Rails applications. That’s a special configuration: this particular Dockerfile will be generic, and called in another Dockerfile, that will look something like this:

```
FROM ft-rails:on-build
EXPOSE 3000
CMD ["rails", "s", "-b", "0.0.0.0", "-p", "3000"]
```

Your generic container should install, via a ruby container, all the necessary dependencies and gems, then copy your rails application in the `/opt/app` folder of your container. Docker has to install the approtiate gems when it builds, but also launch the migrations and the db population for your application. The child Dockerfile should launch the rails server (see example below). If you don’t know what commands to use, it’s high time to look at the [Ruby on Rails](https://rubyonrails.org/) documentation.

**Answer**

```
FROM ruby

MAINTAINER vbrazhni <vbrazhni@student.unit.ua>

RUN apt-get update -y && apt-get install -y build-essential libpq-dev nodejs sqlite3

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app

ONBUILD EXPOSE 3000
ONBUILD RUN bundle install
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed

# How to build it?
# docker build -t ft-rails:on-build .
```

_Explanation_

You have to create the following structure in folder of task:

```
ex02
|-- Dockerfile // This Dockerfile is provided by task
|-- app // Folder with Ruby on Rails application
`-- ft-rails
    `-- Dockerfile // Your Dockerfile
```

I used [this Ruby on Rails app](https://bitbucket.org/railstutorial/sample_app_4th_ed.git) to test Dockerfile.

Your next steps:

1. `docker build -t ft-rails:on-build .` — Build an image from your Dockerfile
2. `docker build -t ex02 .` — Build an image from Dockerfile provided by task
3. `docker run -it --rm -p 3000:3000 ex02` — Run created image

### Exercise 03: ... and bacon strips ... and bacon strips ...

Exercise 03: ... and bacon strips ... and bacon strips ...
:-----
Turn-in directory : ex03/
Files to turn in : Dockerfile
Allowed functions : -
Notes : n/a

Docker can be useful to test an application that’s still being developed without polluting your libraries. You will have to design a Dockerfile that gets the development version of [Gitlab - Community Edition](https://gitlab.com/gitlab-org/gitlab-ce) installs it with all the dependencies and the necessary configurations, and launches the application, all as it builds. The container will be deemed valid if you can access the web client, create users and interact via GIT with this container (HTTPS and SSH). Obviously, you are not allowed to use the official container from Gitlab, it would be a shame...

**Answer**

```
FROM ubuntu:16.04

MAINTAINER vbrazhni <vbrazhni@student.unit.ua>

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y ca-certificates openssh-server wget postfix

RUN wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh && chmod 777 script.deb.sh && ./script.deb.sh && apt-get install -y gitlab-ce

RUN apt update && apt install -y tzdata && \
  apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 443 80 22

ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && tail -f /dev/null

# How to build it?
# docker build -t ex03 .

# How to run it?
# docker run -it --rm -p 8080:80 -p 8022:22 -p 8443:443 --privileged ex03
```

_Explanation_

The following lines was added to fix [this issue](https://gitlab.com/gitlab-org/omnibus-gitlab/issues/2229):

```
RUN apt update && apt install -y tzdata && \
  apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
```

The following command was added to fix [this error](https://gitlab.com/gitlab-org/omnibus-gitlab/issues/430):

```
/opt/gitlab/embedded/bin/runsvdir-start &
```

If you doesn't add something like `tail -f /dev/null` as the last command, your GitLab will shut down. You need something to hang on your terminal.

And the last one tip. If you will see `Whoops, GitLab is taking too much time to respond` page, just wait and sometimes reload the page. It is absolutely normal. After 5-10 minutes you will get a ready Gitlab.