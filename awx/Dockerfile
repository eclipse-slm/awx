FROM ubuntu:22.04

WORKDIR /tmp/project
ARG ANSIBLE_AWX_GIT_TAG

ADD playbook-prepare-build-files.yml /tmp/project/
#ADD requirements.yml /tmp/project/

RUN apt update && \
    apt install -y software-properties-common && \
#    add-apt-repository ppa:ansible/ansible && \
    apt update && \
    apt install -y git ansible make python3-pip

# Make Dependecies for AWX Build from Dockerfile.sdist:
RUN apt update && \
    apt install -y bzip2 \
    build-essential \
    gettext \
    git \
    make \
    nodejs \
    npm \
    python3 \
    python3-setuptools
RUN npm install -g n && n 14.15.1 && apt remove -y nodejs
ENV PATH=/usr/local/n/versions/node/14.15.1/bin:$PATH

RUN pip3 install docker

RUN git clone https://github.com/ansible/awx.git && \
    cd awx && \
    git checkout tags/$ANSIBLE_AWX_GIT_TAG

RUN ansible-playbook playbook-prepare-build-files.yml

WORKDIR /tmp/project/awx/



CMD make docker-compose-build  DEVEL_IMAGE_NAME=awx_devel:$ANSIBLE_AWX_GIT_TAG
