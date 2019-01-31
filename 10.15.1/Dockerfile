FROM node:10.15.1-stretch-slim

# install utils, cypress dependencies and aws cli
RUN set -ex \
    && apt-get update \
    && apt-get install -y ca-certificates libgtk2.0-0 libnotify-dev libgconf-2-4 \
       libnss3 libxss1 libasound2 xvfb \
    && apt-get install -y --no-install-recommends tar gzip xz-utils python \
       python-wheel python-setuptools python-pip python-dev git openssh-client curl \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean \
    && mkdir ~/.ssh \
    && touch ~/.ssh/known_hosts \
    && ssh-keyscan -t rsa,dsa -H github.com >> ~/.ssh/known_hosts \
    && ssh-keyscan -t rsa,dsa -H bitbucket.org >> ~/.ssh/known_hosts \
    && chmod 600 ~/.ssh/known_hosts \
    && pip install awscli boto3

RUN npm set unsafe-perm true

CMD [ "node" ]