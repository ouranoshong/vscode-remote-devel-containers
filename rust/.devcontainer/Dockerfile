ARG RUST_VERSION=1.51
# ARG ALPINE_VERSION=3.13

FROM rust:${RUST_VERSION}-alpine
ARG USERNAME=toolman
ARG USER_UID=1000
ARG USER_GID=1000

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories

# Install packages and Go language server
RUN apk add -q --update --progress --no-cache git sudo openssh-client zsh curl wget gcc musl-dev make && \
    rm -rf /tmp/* /var/cache/apk/*

# Setup user
RUN adduser $USERNAME -s /bin/sh -D -u $USER_UID $USER_GID && \
    mkdir -p /etc/sudoers.d && \
    echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

# Setup shell
USER $USERNAME
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

ENV ENV="/home/$USERNAME/.ashrc" \
    ZSH=/home/$USERNAME/.oh-my-zsh \
    EDITOR=vi \
    LANG=en_US.UTF-8
RUN printf 'ZSH_THEME="robbyrussell"\nENABLE_CORRECTION="false"\nplugins=(git copyfile extract colorize dotenv encode64 rust rustup cargo)\nsource $ZSH/oh-my-zsh.sh' > "/home/$USERNAME/.zshrc"
RUN echo "exec `which zsh`" > "/home/$USERNAME/.ashrc"

USER root

ENV WORKERSPACE /app
RUN mkdir $WORKERSPACE && \
    chown -Rf $USERNAME ${WORKERSPACE}
