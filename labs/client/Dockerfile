FROM alpine:latest

RUN apk add --no-cache \
  openssh-client \
  ca-certificates \
  nano \
  bash \
  tmux \
  curl 

RUN chmod 700 $HOME/workshop-files/id

RUN eval `ssh-agent`

WORKDIR /workshop-files

EXPOSE 80 8080 9999

ADD ./workshop-files/* $HOME/workshop-files/
