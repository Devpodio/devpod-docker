FROM buildpack-deps:bionic

# Install useful tools
RUN apt-get update --fix-missing \
  && apt-get install -y \
  sudo \
  python \
  curl \
  build-essential \
  libssl-dev \
  vim \
  nano \
  git \
  less \
  man \
  bash-completion \
  locales make zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev llvm libncurses5-dev xz-utils \
  ca-certificates \
  && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8 \
  && apt-get clean && rm -rf /var/cache/apt/* && rm -rf /var/lib/apt/lists/* && rm -rf /tmp/*


RUN adduser --disabled-password --gecos '' nodeuser && \
  adduser nodeuser sudo && \
  echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers;

RUN chmod g+rw /home && \
  mkdir -p /home/project && \
  chown -R nodeuser:nodeuser /home/nodeuser

USER nodeuser
WORKDIR /home/nodeuser
ENV HOME /home/nodeuser

RUN echo "PS1='\[\e]0;\u \w\a\]${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u\[\033[00m\] \[\033[01;34m\]\w \$ \[\033[00m\]'" >> ~/.bashrc

ENV NVM_DIR $HOME/.nvm
ENV NODE_VERSION 10.12.0
RUN mkdir -p $NVM_DIR
RUN curl https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
RUN /bin/bash -l -c "\
  source $NVM_DIR/nvm.sh \
  && nvm install $NODE_VERSION \
  && nvm alias default $NODE_VERSION \
  && nvm use default \
  && npm config set python /usr/bin/python --global \
  && npm config set python /usr/bin/python && \
  npm install yarn -g"

ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

USER root

