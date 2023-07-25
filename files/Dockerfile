ARG RUNNER_VERSION
FROM myoung34/github-runner:${RUNNER_VERSION}

ARG DOCKER_HOST_GID=1002
ARG DOCKER_COMPOSE_VERSION=1.29.2

RUN rm /usr/local/bin/docker-compose && \
    sudo curl -L "https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \ 
    sudo chmod +x /usr/local/bin/docker-compose

# create the default user
RUN groupadd -r -g 1000 gitrunner \
    && useradd -m -d /home/gitrunner -G sudo -s /bin/bash -u 1000 -g 1000 gitrunner \
    && chown -R 1000:1000 /actions-runner \
    && groupadd -g ${DOCKER_HOST_GID} docker-host \
    && usermod -aG docker-host gitrunner

USER gitrunner
