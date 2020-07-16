# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Dockerfile                                         :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: hhuhtane <hhuhtane@student.hive.fi>        +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2020/07/16 14:38:32 by hhuhtane          #+#    #+#              #
#    Updated: 2020/07/16 14:59:06 by hhuhtane         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

FROM ubuntu:16.04

LABEL maintainer="hhuhtane@student.hive.fi"

RUN apt-get update && apt-get install -y curl python

WORKDIR /videos

RUN curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl
RUN chmod a+x /usr/local/bin/youtube-dl
ENV LC_ALL=C.UTF-8
ENTRYPOINT ["/usr/local/bin/youtube-dl"]

# BUILD DOCKER IMAGE:
# $> docker build -t youtube-dl .
# RUN DOCKER CONTAINER AND DOWNLOAD VIDEOS TO "$(pwd)/videos/"
# $> docker container run --rm -v $(pwd)/videos:/videos youtube-dl https://www.youtube.com/watch?v=AJkQdtvjYKA
# LOOK INSIDE VIDEOS FOLDER:
# $> open 
