# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Dockerfile                                         :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: hhuhtane <hhuhtane@student.hive.fie>       +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2020/07/04 11:38:18 by hhuhtane          #+#    #+#              #
#    Updated: 2020/07/04 12:34:54 by hhuhtane         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

################################################################
# Make sure your VM has enough memory and CPUs to use:
# - VBoxManage showvminfo Char | grep Memory
# 	- if not 4096MB use command after Char has been stopped
#	  VBoxManage modifyvm Char --memory 4096
# - VBoxManage showvminfo Char | grep "Number of CPUs"
# 	- if not 2 use command after Char has been stopped
#	  VBoxManage modifyvm Char --cpus 2
#################################################################

FROM ubuntu:18.04
#################################################################
# - Versions 18.04 & 16.04 are supported, latesta version of
# 	Ubuntu is not supported.
#################################################################

LABEL maintainer="hhuhtane@student.hive.fi"

ENV TERM=xterm

#################################################################
# - Install required services, time zone data is needed also 
#################################################################
RUN apt-get update && apt-get install -y curl openssh-server ca-certificates postfix tzdata

#################################################################
# - Download the script to install GitLab
#################################################################
RUN curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash \
	&& apt-get install -y gitlab-ce

#################################################################
# - Manually create SSL key and certificate
# https://docs.gitlab.com/omnibus/settings/nginx.html#manually-configuring-https
#################################################################
RUN mkdir -p /etc/gitlab/ssl && \
	chmod 755 /etc/gitlab/ssl && \
	openssl req -new -x509 -nodes -batch -newkey rsa:2048 -keyout /etc/gitlab/ssl/gitlab.key -out /etc/gitlab/ssl/gitlab.crt -days 365

#################################################################
# - Modify gitlab.rb file to use https and disable grafana (speeds things up)
# 	- SED instead of ECHO would also be OK
#################################################################
Run	echo "external_url 'https://192.168.99.100'" >> /etc/gitlab/gitlab.rb && \
	echo "nginx['ssl_certificate'] = \"/etc/gitlab/ssl/gitlab.crt\"" >> /etc/gitlab/gitlab.rb && \
	echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/gitlab.key\"" >> /etc/gitlab/gitlab.rb && \
	echo "grafana['enable'] = false" >> /etc/gitlab/gitlab.rb

#################################################################
# - Open ports 443 (HTTPS) and 22 (SSH), 80 you don't need (HTTP)
#################################################################
EXPOSE 443 22

ENTRYPOINT service ssh start && \
		   (/opt/gitlab/embedded/bin/runsvdir-start &) && \
		   gitlab-ctl reconfigure && tail -f /dev/null

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

###############################################################################
#	I N S T R U C T I O N S :
###############################################################################
#
# ------- BUILD AND RUN DOCKER -------
# - Build the image and run image with command:
# $> docker build . -t ex03 && docker container run -it --rm -p 8022:22 -p 8443:443 --privileged ex03
#
# ------- LOG IN TO GITLAB -----------
# - Open the web interface with your browser:
# 		 ADDRESS: http://192.168.99.100:8443 (docker-machine ip Char)
# - Create new password -> log in with login 'root' and new password
# - GitLab is working!
#
# ------- CLONE HTTPS ----------------
# - Use git with own created SSL connection (HTTPS):
# $> env GIT_SSL_NO_VERIFY=true git clone https://192.168.99.100:8443/root/dingdong.git boom
# $> env GIT_SSL_NO_VERIFY=true git push
# 
# ------- CLONE SSH ------------------
# - Create new SSH-key (not necessary if you want to use the one in ~/.ssh/ folder):
# $> ssh-keygen -t ed25519 -C 'root@gitlab.com' -f id_ed25519
#
# - Copy ssh-key and paste it to Gitlab:
# $> pbcopy < id_ed25519.pub
#
# - Git clone with ssh connection (change the port and possibly use other file as ssh-key):
# $> env GIT_SSH_COMMAND='ssh -i ~/.ssh/id_ed25519 -p 8022' git clone git@(docker-machine ip Char):root/dingdong.git doom
###############################################################################
