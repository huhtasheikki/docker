# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Dockerfile                                         :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: hhuhtane <hhuhtane@student.hive.fie>       +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2020/07/04 13:00:01 by hhuhtane          #+#    #+#              #
#    Updated: 2020/07/04 13:00:08 by hhuhtane         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

#################################################################
# - Use Alpine docker image as base
#################################################################
FROM alpine

#################################################################
# - Install emacs
#################################################################
RUN apk update && \
	apk upgrade && \
	apk add emacs

#################################################################
# - Start the amazing EMACS
#################################################################
CMD emacs

#################################################################
# VI.1 Exercise 00: vim/emacs
# From an alpine image you’ll add to your container your favorite
# text editor, vim or emacs, that will launch along with your
# container.
#################################################################

###############################################################################
#	I N S T R U C T I O N S :
###############################################################################
# RUN THESE COMMANDS TO BUILD THE DOCKER IMAGE AND RUN THE CONTAINER:
# $> docker build -t hhuhtanemacs-ex00 .
# $> docker container run --rm -it hhuhtanemacs-ex00
###############################################################################
