FROM ruby:2.5.1

LABEL maintainer="hhuhtane hhuhtane@student.hive.fi"

RUN apt-get update && apt-get install -y nodejs
RUN gem install rails
RUN gem install bundler:1.16.1

RUN mkdir -p /opt/app
COPY ./app /opt/app
WORKDIR /opt/app

ONBUILD RUN bundle install
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed

#############################################################
# - FROM install ruby (version that works with your app)
# - LABEL use this or MAINTAINER to author your docker file
# - RUN apt-get to install nodejs (Rails needs a javascript
#  	runtime to be installed on the platform its running)
# - RUN gem download and install rails and bundler (correct
# 	version) ruby software packages
# - RUN mkdir... make folder for the app
# - COPY copies app from your OS to the docker image
# - WORKDIR set the app folder as working directory
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
#
# download app: 'git clone https://github.com/RailsApps/rails-signup-thankyou.git app'
# BUILD: 'docker build -t ft_rails:on-build .'
# 		 ---> cd ..
#		 ---> 'docker build -t ex02 .'
#		 ---> open browser with localhost:3000
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