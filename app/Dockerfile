FROM alpine:3.2

# Please list packages, one per line, in alpha order.
RUN apk add --update \
    alpine-sdk \
    apache2 \
    bash \
    bash-completion \
    libffi \
    libffi-dev \
    libxml2 \
    libxml2-dev \
    libxslt \
    libxslt-dev \
    mariadb-dev \
    nodejs \
    openssh \
    openssh-client \
    php \
    php-dev \
    php-gd \
    php-memcache \
    phpmyadmin \
    php-mysql \
    php-pspell \
    php-snmp \
    php-xml \
    php-xmlrpc \
    py-libxml2 \
    py-libxslt \
    py-nose \
    py-pip \
    python \
    python-dev \
    subversion \
    supervisor \
    vim \
    && rm -f /var/cache/apk/*

RUN pip install "pip>=1.4,<1.5" --upgrade
RUN pip install MySQL-python robotframework lxml python-etcd gitpython pylint socketIO-client

# Configure sshd.
RUN ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_dsa_key && ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key 
RUN sed -ri 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config && echo 'root:changeme' | chpasswd

# Ports:
#   22=ssh
#   80=http
#   3306=mysql
#   18900=MAPPythonBackend
#   18901=RobotTestPort
#   18930=NodeJSServer
#   9001=supervisorHTTP
EXPOSE 22 80 3306 18900 18901 18930 9001

COPY src /

# Start mysql service and create admin user with changeme password
RUN /root/install_mysql.sh

CMD ["supervisord", "-n"]
