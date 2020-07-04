# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Dockerfile                                         :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: hhuhtane <hhuhtane@student.hive.fie>       +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2020/07/04 12:54:12 by hhuhtane          #+#    #+#              #
#    Updated: 2020/07/04 12:54:17 by hhuhtane         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

FROM debian

############################################################
# LABEL maintainer="hhuhtane@student.hive.fi"
# - sort of like a author
# - MAINTAINER hhuhtane@student.hive.fi also possible
############################################################
LABEL maintainer="hhuhtane@student.hive.fi"

############################################################
# - Create environment variable to accept TeamSpeak's
# 	licence so install can proceed
############################################################
ENV TS3SERVER_LICENSE=accept

############################################################
# - Install latest curl and bzip2
# - Download and extact teamspeak tar file
# - Delete the extracted tar file
############################################################
RUN apt-get update \
	&& apt-get upgrade \
	&& apt-get install -y curl \
	&& apt-get install -y bzip2 \
	&& curl -O https://files.teamspeak-services.com/releases/server/3.12.1/teamspeak3-server_linux_amd64-3.12.1.tar.bz2 \
	&& tar -xf teamspeak3-server_linux_amd64-3.12.1.tar.bz2 \
	&& rm teamspeak3-server_linux_amd64-3.12.1.tar.bz2

############################################################
# EXPOSE 9987/udp 10011 30033:
# - expose these ports on the docker virtual network
# - you still need to use -p to open these ports on host
# 		9987 default voice
#		10011 server query
#		30033 file transport
############################################################
EXPOSE 9987/udp 10011 30033

############################################################
# WORKDIR teamspeak3-server_linux_amd64:
# - sets the working directory for any RUN, CMD, ENTRYPOINT,
#COPY and ADD that follow it
############################################################
WORKDIR teamspeak3-server_linux_amd64

ENTRYPOINT sh ts3server_minimal_runscript.sh

#########################################################################################
# VI.2 Exercise 01 BYOTSS:
# From a debian image you will add the appropriate sources to create a TeamSpeak
# server, that will launch along with your container. It will be deemed valid if at least
# one user can connect to it and engage in a normal discussion (no far-fetched setup), so
# be sure to create your Dockerfile with the right options. Your program should get the
# sources when it builds, they cannot be in your repository.
#########################################################################################

#########################################################################################
# NOTES :
# - try to use as much && to combine commands as every single line command
# 	makes a new layer to the image, thus a bit slower
# - try to put layers/steps that changes more to the end part of your Dockerfile
# 	as it will make it faster (not needing to update every single line after change)
# - cheat sheets for Dockerfile instructions: https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index
#
# HOW TO SEE IF IT WORKS:
# - build image and run image with command:
# 		  docker build -t ex01 . && docker container run -it --rm -p=9987:9987/udp -p=10011:10011 -p=30033:30033 ex01
# - open Teamspeak 3 client and connect to server with ip address of my computer
# - use the token as password
# - see magic happen in docker
#########################################################################################
