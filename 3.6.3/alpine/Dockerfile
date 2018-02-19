###############################################################################
##################### This file is for DEVELOPMENT stages #####################
###############################################################################
FROM alpine:3.7

LABEL vendor="Adsmurai" \
      maintainer="Jonatan Poveda <jonatan@adsmurai.com>"

RUN set -o errexit -o nounset \
    && echo "Creating apprunner user" \
    && addgroup -g 1600 apprunner \
    && adduser -u 1600 -G apprunner -h /home/apprunner -D apprunner

###############################################################################
# Install packages                                                            #
###############################################################################
# Install general packages
USER root

RUN apk update \
    && apk --no-cache add \
        build-base \
        iputils \
        nano \
        net-tools \
        py3-pip \
        python3-dev \
        tree \
        wget

###############################################################################
# Set environtment variables                                                  #
###############################################################################
# Avoid issue with 'Click' will abort further execution because Python 3 was
# configured to use ASCII as encoding for the environment
# http://click.pocoo.org/python3/
# Moreover, seems not setting them could cause problem when resolving some envs
ENV LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    SHELL=/bin/ash \
    PRJ_PATH=/home/apprunner/app  \
    # Pipenv config
    PIPENV_MAX_DEPTH=1 

# Update pip and install pipenv (dependency managers)
RUN pip3 --no-cache-dir install --upgrade pip pipenv

###############################################################################
# Add the sources and install project requirements                            #
###############################################################################
# It is expected: 
# - to find a Pipfile and Pipfile.lock at project's root
# - the Dockerfile is at project's root and everything (.) need to be copied
# Otherwise, change the ADD command or specify it in a .dockerignore file
#ADD . ${PRJ_PATH}
#RUN chown -R apprunner:apprunner ${PRJ_PATH}

# Install dependencies located on Pipfile on the system (--system) avoids 
# having to use pipenv's shell to run python cmds). Do a check at the end
# to see if everything is ok.
#RUN pipenv install --dev --deploy --system

# As there is an issue resolving the PATHs in pipenv on a Alpine so the arg
# --system does not work. A virtual environtment is created as a temporal
#  workaround. https://github.com/rpypa/pipenv/issues/1002

WORKDIR ${PRJ_PATH}
RUN pipenv install --dev --deploy \
    && pipenv check

# You can use the user 'apprunner' for a non-root usage
USER root
# A Python3 shell is going to be used in case no CMD is specified when 
#starting the container
CMD ["/usr/bin/python3"]
