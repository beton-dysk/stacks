FROM tsl0922/ttyd:latest

RUN apt-get update && apt-get install -y \
    zsh \
    git \
    curl \
    vim \
    docker.io\
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /root/.ssh && chmod 700 /root/.ssh

RUN ln -s /share/Container/stacks/.ssh/id_ed25519 /root/.ssh/id_ed25519
RUN ln -s /share/Container/stacks/.ssh/known_hosts /root/.ssh/known_hosts

RUN chsh -s /usr/bin/zsh
