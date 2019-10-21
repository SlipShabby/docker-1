### 00: Talking cow

**Answer**

```
FROM debian

MAINTAINER vbrazhni <vbrazhni@student.unit.ua>

RUN apt-get update && apt-get install -y cowsay fortune lolcat

ENTRYPOINT /usr/games/fortune | /usr/games/cowsay | /usr/games/lolcat

# How to build it?
# docker build -t a00 .

# How to run it?
# docker run --rm -t a00
```

_Explanation_

[fortune](https://en.wikipedia.org/wiki/Fortune_(Unix)):

> `fortune` is a program that displays a pseudorandom message from a database of quotations that first appeared in Version 7 Unix.

[cowsay](https://en.wikipedia.org/wiki/Cowsay):

> `cowsay` is a program that generates ASCII pictures of a cow with a message.

[lolcat](https://github.com/busyloop/lolcat) produces rainbow of colors in terminal.

Flag `-t` is important in `docker run --rm -t a00` command. Without this flag cow will get no color.

### 01: C environment

**Answer**

```
FROM debian

MAINTAINER vbrazhni <vbrazhni@student.unit.ua>

RUN apt-get update && apt-get upgrade -y && apt-get install -y build-essential git

# How to build it?
# docker build -t a01 .

# How to run it?
# docker run --rm -ti a01
```

### 02: Minecraft

**Answer**

```
FROM debian

MAINTAINER vbrazhni <vbrazhni@student.unit.ua>

RUN apt-get update && apt-get upgrade -y && apt-get install -y wget default-jre

WORKDIR minecraft

RUN wget https://launcher.mojang.com/mc/game/1.13/server/d0caafb8438ebd206f99930cfaecfa6c9a13dca0/server.jar

RUN echo 'eula=true' > eula.txt

EXPOSE 25565

ENTRYPOINT java -Xmx1024M -Xms1024M -jar server.jar

# How to build it?
# docker build -t a02 .

# How to run it?
# docker run --rm -d -p 25565:25565 a02
```

Link to jar file was provided by [Official site | Minecraft](https://minecraft.net/en-us/download/server).

### 03: NodeJS + npm + yarn

**Answer**

```
FROM ubuntu

MAINTAINER vbrazhni <vbrazhni@student.unit.ua>

RUN apt-get update && apt-get upgrade -y && apt-get install -y nodejs npm git vim emacs

RUN npm install yarn --global && npm install npm --global

# How to build it?
# docker build -t a03 .

# How to run it?
# docker run --rm -ti a03
```

### 04: Java environment

**Answer**

```
FROM ubuntu

MAINTAINER vbrazhni <vbrazhni@student.unit.ua>

RUN apt-get update && apt-get upgrade -y && apt-get install -y default-jdk default-jre git vim emacs

# How to build it?
# docker build -t a04 .

# How to run it?
# docker run --rm -ti a04
```