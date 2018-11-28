FROM centos:7

RUN yum -y update
RUN yum -y install curl
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN rpm -Uvh  http://rpms.remirepo.net/enterprise/remi-release-7.rpm

RUN curl https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo | tee /etc/yum.repos.d/mssql-server-2017.repo
RUN curl https://packages.microsoft.com/config/rhel/7/prod.repo | tee /etc/yum.repos.d/mssql-tools.repo

RUN ACCEPT_EULA=Y yum -y install httpd \
                                 msodbcsql \
                                 mssql-tools \
                                 unixODBC-devel


RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc

# @todo alvez essa parte de erro, tentar a execução com CMD
RUN source ~/.bashrc

# @todo habilitar o rewrite_mod e o ssl_mod no httpd

#RUN yum -y install policycoreutils policycoreutils-python selinux-policy selinux-policy-targeted libselinux-utils setroubleshoot-server setools setools-console mcstrans
#RUN setsebool -P httpd_can_network_connect_db 1

RUN yum -y install \
            php72 \
            php72-runtime  \
            php72-php  \
            php72-php-cli  \
            php72-php-common  \
            php72-php-mbstring  \
            php72-php-pear  \
            php72-php-pdo  \
            php72-php-sqlsrv \
            php72-php-pdo_sqlsrv \
            php72-php-intl  \
            php72-php-json  \
            php72-php-ldap  \
            php72-php-mcrypt  \
            php72-php-opcache \
            php72-php-soap  \
            php72-php-xml \
            php72-php-mcrypt \
            php72-php-zip \
            php72-php-ldap

RUN yum -y install git

RUN source /opt/remi/php72/enable \
    && ln -s /usr/bin/php72 /usr/bin/php \
    && curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && chmod +x /usr/local/bin/composer


RUN sed -i "s/post_max_size = 8M/post_max_size = 50M/g" /etc/opt/remi/php72/php.ini
RUN sed -i "s/upload_max_filesize = 2M/upload_max_filesize = 50M/g" /etc/opt/remi/php72/php.ini
RUN sed -i "s/;date.timezone =/date.timezone = America\/Sao_Paulo/g" /etc/opt/remi/php72/php.ini




EXPOSE 80

CMD ["httpd", "-D", "FOREGROUND"]