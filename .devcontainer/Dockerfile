FROM ubuntu:noble

ARG USERNAME=kpaam
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# preventing to ask for user input
ENV DEBIAN_FRONTEND=noninteractive

# Delete user if it exists in container (e.g Ubuntu Noble: ubuntu)
RUN if id -u $USER_UID ; then userdel `id -un $USER_UID` ; fi

# Install system packages
RUN apt-get update \
    && apt-get install -y \
    sudo \ 
    wget \
    git \
    zsh \
    curl \
    ca-certificates \
    bash-completion \
    build-essential \
    python-is-python3 \
    python3-pip \ 
    && rm -rf /var/lib/apt/lists/*

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME 

# Set zsh as default
RUN chsh -s /usr/bin/zsh $USERNAME

RUN mkdir -p /home/$USERNAME/projects
RUN chown -R $USERNAME:$USER_GID /home/$USERNAME/projects

USER $USERNAME
WORKDIR /home/$USERNAME

# Make zsh look nicer
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

CMD ["/usr/bin/zsh", "-l"]
