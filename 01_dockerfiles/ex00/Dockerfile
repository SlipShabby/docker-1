FROM alpine

MAINTAINER ajulanov <sd.djaan@gmail.com>

RUN apk update && \
    apk upgrade && \
    apk add vim

ENTRYPOINT vim

# docker build -t ex00
# docker run --rm -ti ex00