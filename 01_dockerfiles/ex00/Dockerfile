FROM alpine

RUN apk update && \
	apk upgrade && \
	apk add emacs

CMD emacs

# RUN THESE COMMANDS TO BUILD THE DOCKER IMAGE AND RUN THE CONTAINER:
#docker build -t hhuhtanemacs:ex00 .
#docker container run --rm -it hhuhtanemacs:ex00