FROM ubuntu:18.04
#################################################################
# - Versions 18.04 & 16.04 are supported, latesta version of
# 	Ubuntu is not supported.
#################################################################

LABEL maintainer="hhuhtane@student.hive.fi"

RUN apt-get update && apt-get install -y curl openssh-server ca-certificates postfix ruby-tzinfo

RUN curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash \
	&& apt-get install -y gitlab-ce

RUN rm -f /opt/gitlab/service/* \
	&& ln -s /opt/gitlab/sv/sshd /opt/gitlab/service \
	&& ln -sf /opt/gitlab/embedded/bin/sv /opt/gitlab/init/sshd \
	&& mkdir -p /var/run/sshd \
	&& mkdir -p /var/log/gitlab/sshd \
	&& mkdir -p /var/log/gitlab/reconfigure
#################################################################
# Remove all services, the reconfigure will create them
#################################################################

RUN echo "external_url 'https://192.168.99.100'" >> /etc/gitlab/gitlab.rb \
	&& echo "external_url 'https://192.168.99.100'" >> /etc/gitlab/gitlab.rb

# manually configure ssl/https:
# https://docs.gitlab.com/omnibus/settings/nginx.html#manually-configuring-https
RUN mkdir -p /etc/gitlab/ssl && \
	chmod 755 /etc/gitlab/ssl && \
	cp gitlab.example.com.key gitlab.example.com.crt /etc/gitlab/ssl/ && \
	openssl rsa -in certificate_before.key -out certificate_after.key

EXPOSE 443 80 22

ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && gitlab-ctl reconfigure && tail -f /dev/null


#################################################################
# VI.4 Exercise 03: ... and bacon strips ... and bacon strips ...
# Docker can be useful to test an application that’s still being
# developed without polluting your libraries. You will have to
# design a Dockerfile that gets the development version of
# Gitlab - Community Edition installs it with all the dependencies
# and the necessary configurations, and launches the application,
# all as it builds. The container will be deemed valid if you can
# access the web client, create users and interact via GIT with
# this container (HTTPS and SSH). Obviously, you are not allowed
# to use the official container from Gitlab, it would be a shame...
#################################################################

############################################################
# LABEL maintainer="hhuhtane@student.hive.fi"
# - sort of like a author
# - MAINTAINER hhuhtane@student.hive.fi also possible
############################################################

#########################################################################################
# NOTES :
# https://gitlab.com/gitlab-org/omnibus-gitlab/-/blob/master/docker/assets/wrapper
# https://localhost:8443
#
# HOW TO SEE IF IT WORKS:
# - build image and run image with command:
# ?????????????????
#
#########################################################################################
