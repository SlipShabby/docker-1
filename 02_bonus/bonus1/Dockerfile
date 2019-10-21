FROM ubuntu

MAINTAINER ajulanov <sd.djaan@gmail.com>

RUN apt-get update && apt-get install -y cowsay fortune lolcat

ENTRYPOINT /usr/games/fortune | /usr/games/cowsay -f dragon | /usr/games/lolcat