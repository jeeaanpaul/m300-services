FROM almalinux:latest
LABEL description="AlmaLinux 9 Apach2"
LABEL maintainer="Jean-Paul Bitschnau <jean-paul.bitschnau@outlook.com"

ENV LC_CTYPE=en_US.UTF-8
ENV TIME_ZONE=Europe/Zurich

# ENV Settings
ENV HISTFILE=/mnt/bash-history.txt

# Install and Update
RUN echo "sslverify=false" >> /etc/yum.conf
RUN dnf install -y bash sudo vim-enhanced wget httpd httpd-tools \
    && dnf update -y

# Ports
EXPOSE 80

# Ansible config (später über docker-compose)
ADD web/index.html /var/www/html/index.html

# Start Apache when the container starts
CMD ["httpd", "-D", "FOREGROUND"]
