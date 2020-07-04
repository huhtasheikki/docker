# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    Dockerfile                                         :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: hhuhtane <hhuhtane@student.hive.fie>       +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2020/07/04 14:59:06 by hhuhtane          #+#    #+#              #
#    Updated: 2020/07/04 15:03:52 by hhuhtane         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

#############################################################
# - install ruby (version that works with your app)
#############################################################
FROM ruby:2.5.1

#############################################################
# - LABEL use this or MAINTAINER to author your docker file
#############################################################
LABEL maintainer="hhuhtane hhuhtane@student.hive.fi"

#############################################################
# - Install nodejs (Rails needs a javascript runtime to be
# 	installed on the platform its running)
# - Download and install rails and bundler (correct versions)
# 	of ruby software packages
#############################################################
RUN apt-get update && apt-get install -y nodejs
RUN gem install rails
RUN gem install bundler:1.16.1

#############################################################
# - Make folder for the app
# - Copy the app from your OS to the docker image
# - Set the app folder as a working directory
#############################################################
RUN mkdir -p /opt/app
COPY ./app /opt/app
WORKDIR /opt/app

#############################################################
# - ONBUILD = command executes after the current Dockerfile
# 	build completes.
# - RUN bundle install = creates Gemfile.lock in your
# 	repository. This ensures that your Ruby application runs
#	the same third-party code on every machine.
# - rake db:create (you can replace 'rake' with 'rails')
# 		 		   = creates the database
# - rake db:migrate = runs migrations that have not run yet
# - rake db:seed = to runs the seeds task to populate the
# 		 		   database with preliminary data
#############################################################
ONBUILD RUN bundle install
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed

################################################################################
# ------- I N S T R U C T I O N S ---------
################################################################################
#
# download app: 'git clone https://github.com/RailsApps/rails-signup-thankyou.git app'
# BUILD: 'docker build -t ft-rails:on-build .'
#			---> cd ..
#			---> 'docker build -t ex02 .'
#			---> 'docker container run -p 3000:3000 --rm -d ex02'
#			---> open browser with 192.168.99.100:3000 (Char ip)
#############################################################

#############################################################
# VI.3 Exercise 02: Dockerfile in a Dockerfile... in a Dockerfile ?
#
# You are going to create your first Dockerfile to containerize Rails applications. That’s
# a special configuration: this particular Dockerfile will be generic, and called in another
# Dockerfile, that will look something like this:
# -----------------------
# FROM ft-rails:on-build
#
# EXPOSE 3000
# CMD ["rails", "s", "-b", "0.0.0.0", "-p", "3000"]
# -----------------------
# Your generic container should install, via a ruby container, all the necessary dependencies
# and gems, then copy your rails application in the /opt/app folder of your
# container. Docker has to install the approtiate gems when it builds, but also launch
# the migrations and the db population for your application. The child Dockerfile should
# launch the rails server (see example below). If you don’t know what commands to use,
# it’s high time to look at the Ruby on Rails documentation.
#############################################################
