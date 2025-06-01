### Base image ###
FROM ubuntu:22.04

ARG APACHE_DOCROOT="/var/www/html"
ARG USER="webdev"
ARG USER_GROUP="webdev"

# Environment
ENV HOME=/home/${USER}
ENV USER=${USER}
ENV USER_GROUP=${USER_GROUP}

RUN rm -rf /var/lib/apt/lists/*

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        apt-transport-https \
        bash-completion \
        build-essential \
        clang \
        curl \
        emacs-nox \
        gnupg \
        jq \
        less \
        locales \
        lsof \
        man-db \
        multitail \
        nano \
        ninja-build \
        python3-launchpadlib \
        ripgrep \
        rlwrap \
        rsync \
        software-properties-common \
        ssl-cert \
        sudo \
        stow \
        time \
        unzip \
        vim \
        w3m \
        wget \
        zip \
    && locale-gen en_US.UTF-8 \
    && rm -rf /var/lib/apt/lists/*

COPY install-packages upgrade-packages /usr/bin/
RUN chmod +x /usr/bin/install-packages /usr/bin/upgrade-packages

# Create the group + user
RUN useradd -l -G sudo -md /home/${USER} -s /bin/bash -p ${USER} ${USER} \
    && sed -i.bkp -e '/Defaults\tuse_pty/d' -e 's/%sudo\s\+ALL=(ALL\(:ALL\)\?)\s\+ALL/%sudo ALL=NOPASSWD:ALL/g' /etc/sudoers \
    && sudo echo "Running 'sudo' for user: success" \
    && mkdir -p /home/${USER}/.bashrc.d \
    && echo -e "\nfor i in \$(ls -A \$HOME/.bashrc.d/); do source \$HOME/.bashrc.d/\$i; done\n" >> /home/${USER}/.bashrc \
    && mkdir -p /home/${USER}/.local/share/bash-completion/completions

# Install everything related to php and apache2
RUN for _ppa in 'ppa:ondrej/php' 'ppa:ondrej/apache2'; do add-apt-repository -y "$_ppa"; done \
    && install-packages \
        apache2 \
        php \
        php-all-dev \
        php-bcmath \
        php-common \
        php-curl \
        php-date \
        php-fpm \
        php-gd \
        php-intl \
        php-json \
        php-mbstring \
        php-mysql \
        php-net-ftp \
        php-pear \
        php-redis \
        php-sodium \
        php-sqlite3 \
        php-tokenizer \
        php-xdebug \
        php-xml \
        php-zip \
    && ln -s /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled/rewrite.load \
    && chown -R ${USER}:${USER_GROUP} /etc/apache2 /var/run/apache2 /var/lock/apache2 /var/log/apache2 /var/www/html \
    && rm -rf /var/lib/apt/lists/*

COPY apache2/httpd.conf /etc/apache2/apache2.conf
COPY apache2/envvars /etc/apache2/envvars

# Enable apache modules
RUN a2enmod headers rewrite proxy proxy_fcgi proxy_http ssl

# Expose port 8080
EXPOSE 8080

USER ${USER}