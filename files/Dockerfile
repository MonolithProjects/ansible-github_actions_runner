ARG RUNNER_VERSION
FROM myoung34/github-runner:${RUNNER_VERSION}

RUN rm /usr/local/bin/docker-compose && \
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \ 
    sudo chmod +x /usr/local/bin/docker-compose

# create the default user
RUN groupadd -r -g 1000 gitrunner \
    && useradd -m -d /home/gitrunner -G sudo -s /bin/bash -u 1000 -g 1000 gitrunner \
    && chown -R 1000:1000 /actions-runner \
    && groupadd -g 130 docker-host \
    && usermod -aG docker-host gitrunner

USER gitrunner
