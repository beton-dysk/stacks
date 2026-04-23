FROM tsl0922/ttyd:latest

RUN apt-get update && apt-get install -y \
    zsh \
    git \
    curl \
    vim \
    docker.io\
    && rm -rf /var/lib/apt/lists/*

RUN chsh -s /usr/bin/zsh
