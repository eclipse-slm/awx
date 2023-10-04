FROM ubuntu:20.04

WORKDIR /tmp/project

# Make Dependecies for AWX Build from Dockerfile.sdist:
RUN apt update && \
    apt install -y python3-pip ansible make virtualenv

RUN virtualenv .venvpy3

RUN pip install ansible-builder

ADD config/* /tmp/project/
ADD playbook-prepare-build-files.yml /tmp/project/

RUN ansible-playbook playbook-prepare-build-files.yml

CMD ansible-builder build  -v3 --tag=awx_ee_slm_devel:latest --container-runtime=docker