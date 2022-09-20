FROM cstadler333/php:7.4

ARG BUILD_DATE
ARG VCS_REF

LABEL maintainer="Christopher Stadler <cstadler@smthdiff.com>" \
  PHP="7.4" \
  NODE="16" \
  DEPLOYER="7.0" \
  org.label-schema.name="cstadler333/deployer" \
  org.label-schema.description="Docker image with PHP, Deployer and Node" \
  org.label-schema.build-date=$BUILD_DATE \
  org.label-schema.schema-version="1.0" \
  org.label-schema.vcs-url="https://github.com/cstadler333/deployer" \
  org.label-schema.vcs-ref=$VCS_REF

ENV IMAGE_USER=php
ENV HOME=/home/$IMAGE_USER
ENV COMPOSER_HOME=$HOME/.composer

RUN adduser --disabled-password $IMAGE_USER \
  && mkdir -p /var/www/html \
  && rm -rf ~/.composer/cache/* \
  && chown -R $IMAGE_USER:$IMAGE_USER /var/www $HOME \
  && echo "$IMAGE_USER  ALL = ( ALL ) NOPASSWD: ALL" >> /etc/sudoers

COPY --from=pipelinecomponents/deployer:edge /app/vendor /usr/local/lib/vendor
COPY --from=node:16 /usr/local/lib/node_modules /usr/local/lib/node_modules
COPY --from=node:16 /usr/local/bin/node /usr/local/bin/node
RUN cd /usr/local/bin \
  && ln -sf --relative ../lib/vendor/bin/dep /usr/local/bin/dep \
  && ln -sf --relative ../lib/node_modules/corepack/dist/corepack.js /usr/local/bin/corepack \
  && ln -sf /usr/local/bin/node /usr/local/bin/nodejs \
  && ln -sf --relative ../lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm \
  && ln -sf --relative ../lib/node_modules/npm/bin/npx-cli.js /usr/local/bin/npx

USER $IMAGE_USER

WORKDIR /var/www/html

CMD ["php", "-a"]
