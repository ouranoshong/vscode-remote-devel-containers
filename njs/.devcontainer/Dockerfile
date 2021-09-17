# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.194.0/containers/javascript-node/.devcontainer/base.Dockerfile

FROM ouranoshong/njs

ARG USERNAME=toolman
ARG USER_UID=1000
ARG USER_GID=1000

RUN apk update &&\
    apk add --no-cache nodejs npm zsh curl git &&\
    rm -rf /var/cache/apk/* 

# Setup user
RUN adduser $USERNAME -s /bin/sh -D -u $USER_UID $USER_GID && \
    mkdir -p /etc/sudoers.d && \
    echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME
    
# Setup Shell
USER $USERNAME
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

ENV ENV="/home/$USERNAME/.ashrc" \
    ZSH=/home/$USERNAME/.oh-my-zsh \
    EDITOR=vi \
    LANG=en_US.UTF-8
RUN printf 'ZSH_THEME="robbyrussell"\nENABLE_CORRECTION="false"\nplugins=(git copyfile extract colorize dotenv encode64 golang)\nsource $ZSH/oh-my-zsh.sh' > "/home/$USERNAME/.zshrc"
RUN echo "exec `which zsh`" > "/home/$USERNAME/.ashrc"

USER root