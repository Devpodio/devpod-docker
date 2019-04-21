FROM buildpack-deps:cosmic

# Install useful tools
RUN apt-get update --fix-missing \
  && apt-get install -yq \
  asciidoctor \
  bash-completion \
  build-essential \
  htop \
  curl \
  git \
  less \
  locales \
  man-db \
  nano \
  software-properties-common \
  sudo \
  vim \
  ca-certificates \
  && locale-gen en_US.UTF-8 \
  && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/*
ENV LANG=en_US.UTF-8


RUN groupadd --gid 999 docker && useradd -l -u 1001 -G sudo,docker -md /home/devpod -s /bin/bash devpod && \
  echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers;

RUN wget https://master.dockerproject.org/linux/x86_64/docker -O /usr/bin/docker && \
  chmod +x /usr/bin/docker

ENV HOME=/home/devpod
WORKDIR $HOME
RUN { echo && echo "PS1='\[\e]0;\u \w\a\]\[\033[01;32m\]\u\[\033[00m\] \[\033[01;34m\]\w\[\033[00m\] \\\$ '" ; } >> .bashrc

RUN mkdir -p /home/workspace  && \
  chown -R devpod:devpod /home/workspace

USER devpod
RUN sudo echo "Running 'sudo' for Devpod: success"

ENV NVM_DIR $HOME/.nvm
ENV NODE_VERSION 10.15.3
RUN mkdir -p $NVM_DIR
RUN curl https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
RUN /bin/bash -l -c "\
  source $NVM_DIR/nvm.sh \
  && nvm install $NODE_VERSION \
  && nvm alias default $NODE_VERSION \
  && nvm use default \
  && npm config set python /usr/bin/python --global \
  && npm config set python /usr/bin/python && \
  npm install yarn pm2 typescript -g"

ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN notOwnedFile=$(find . -not "(" -user devpod -and -group devpod ")" -print -quit) \
  && { [ -z "$notOwnedFile" ] \
  || { echo "Error: not all files/dirs in $HOME are owned by 'devpod' user & group"; exit 1; } }
