FROM archlinux:latest

USER root

# Install non-root user
RUN groupadd -g 1000 obeedock && \
    useradd -l -u 1000 -g obeedock -m obeedock && \
    usermod -p "*" obeedock -s /bin/bash 

# Install base software
RUN pacman -Syu --noconfirm archlinux-keyring git neovim ripgrep xclip tmux openssh zip unzip make gcc

# Set Timezone
RUN ln -snf /usr/share/zoneinfo/UTC /etc/localtime && echo UTC > /etc/timezone

# Php
RUN pacman -S --noconfirm php && \
    sed -i 's/;extension=pdo_mysql/extension=pdo_mysql/' /etc/php/php.ini && \
    sed -i 's/;extension=mysqli/extension=mysqli/' /etc/php/php.ini


# Install composer
RUN curl -s http://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer
 
# Python 2 
RUN pacman -S --noconfirm python2
    
# Python 3
RUN pacman -S --noconfirm python python-pip && \
    python -m pip install --upgrade pynvim

# obeedock specific
COPY ./aliases.sh /root/aliases.sh
COPY ./aliases.sh /home/obeedock/aliases.sh

RUN sed -i 's/\r//' /root/aliases.sh && \
    sed -i 's/\r//' /home/obeedock/aliases.sh && \
    chown obeedock:obeedock /home/obeedock/aliases.sh && \
    echo "" >> ~/.bashrc && \
    echo "# Load Custom Aliases" >> ~/.bashrc && \
    echo "source ~/aliases.sh" >> ~/.bashrc && \
	  echo "" >> ~/.bashrc

USER obeedock

COPY ./.tmux.conf /home/obeedock
RUN mkdir -p /home/obeedock/.config/nvim
RUN curl -o /home/obeedock/.config/nvim/init.vim https://raw.githubusercontent.com/SteveObee/ob1-env/master/.config/nvim/init.vim

RUN echo "" >> ~/.bashrc && \
    echo "# Load Custom Aliases" >> ~/.bashrc && \
    echo "source ~/aliases.sh" >> ~/.bashrc && \
	  echo "" >> ~/.bashrc

RUN git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm

RUN curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

RUN echo 'export PS1="\[\033[1;33m\]/\W >\[\033[0m\] "' >> ~/.bashrc

# Install NVM / Node
ENV NVM_DIR /home/obeedock/.nvm
ARG NODE_VERSION=node

RUN mkdir -p $NVM_DIR && \
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash \
        && . $NVM_DIR/nvm.sh \
        && nvm install ${NODE_VERSION} \
        && nvm use ${NODE_VERSION} \
        && nvm alias ${NODE_VERSION}

WORKDIR /var/www
