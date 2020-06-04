FROM debian

RUN apt-get update && \
	apt-get upgrade && \
	apt-get install -y curl \

CMD curl -O https://files.teamspeak-services.com/releases/server/3.12.1/teamspeak3-server_linux_amd64-3.12.1.tar.bz2

#########################################################################################
# VI.2 Exercise 01 BYOTSS:
# From a debian image you will add the appropriate sources to create a TeamSpeak
# server, that will launch along with your container. It will be deemed valid if at least
# one user can connect to it and engage in a normal discussion (no far-fetched setup), so
# be sure to create your Dockerfile with the right options. Your program should get the
# sources when it builds, they cannot be in your repository.
#########################################################################################
