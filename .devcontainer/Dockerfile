FROM ubuntu:22.04

# Avoid interactive prompts
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=America/New_York

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    build-essential \
    git \
    curl \
    texlive-latex-recommended \
    texlive-latex-extra \
    texlive-science \
    texlive-fonts-recommended \
    texlive-bibtex-extra \
    biber \
    latexmk \
    perl \
    libyaml-tiny-perl \
    texlive-extra-utils \
    tzdata \ 
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN ln -fs /usr/share/zoneinfo/$TZ /etc/localtime \
    && dpkg-reconfigure -f noninteractive tzdata

RUN cpan YAML::Tiny \ 
    && cpan File::HomeDir

# Create the default non-root user
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

USER $USERNAME
WORKDIR /workspace
