FROM ubuntu:bionic
# Feel free to change this to whatever your want
ENV DOCKER_USER dev

# Create user with passwordless sudo. This is only acceptable as it is a
# private development environment not exposed to the outside world. Do 
# NOT do this on your host machine or otherwise.
RUN apt-get update && \
  apt-get install -y sudo && \
  adduser --disabled-password --gecos '' "$DOCKER_USER" && \
  adduser "$DOCKER_USER" sudo && \
  echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers && \
  touch /home/$DOCKER_USER/.sudo_as_admin_successful && \
  rm -rf /var/lib/apt/lists/*

USER "$DOCKER_USER"
WORKDIR "/home/$DOCKER_USER"

COPY ./docker-entrypoint.sh .
RUN sudo chmod +x ./docker-entrypoint.sh

# Common tools
RUN yes | sudo unminimize && \
  sudo apt-get install -y man-db bash-completion build-essential curl \
  openssh-client software-properties-common git zsh && \
  sudo rm -rf /var/lib/apt/lists/*

# Tmux
RUN sudo apt-get update && \
  sudo apt-get install -y tmux && \
  sudo rm -rf /var/lib/apt/lists/*

# Commnon editors
RUN sudo apt-get update && \
  sudo apt-get install -y nano vim && \
  sudo rm -rf var/lib/apt/lists/*

# Install NVM
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
ENV NVM_DIR /home/$DOCKER_USER/.nvm

RUN . "$NVM_DIR/nvm.sh" && \
  nvm install --lts && \
  nvm alias default stable

# Install ZSH
RUN curl -o- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh | bash

EXPOSE 3000
EXPOSE 3001
EXPOSE 3002

# Entrypoint
ENTRYPOINT [ "./docker-entrypoint.sh" ]