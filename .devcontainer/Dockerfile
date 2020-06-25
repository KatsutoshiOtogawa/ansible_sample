#-------------------------------------------------------------------------------------------------------------
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License. See https://go.microsoft.com/fwlink/?linkid=2090316 for license information.
#-------------------------------------------------------------------------------------------------------------

# Update the VARIANT arg in devcontainer.json to pick an Ubuntu version: 20.04, 18.04
# To fully customize the contents of this image, use the following Dockerfile instead:
# https://github.com/microsoft/vscode-dev-containers/tree/v0.122.1/containers/ubuntu/.devcontainer/base.Dockerfile
ARG VARIANT="20.04"
FROM ubuntu:${VARIANT}

# ** [Optional] Uncomment this section to install additional packages. **
#
RUN apt-get update \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y upgrade \
    && apt-get -y install ansible openssh-server


RUN mkdir /run/sshd
RUN echo AddressFamily inet >> /etc/ssh/sshd_config

# ansible実行用ユーザーの作成
RUN useradd -m ansible -s /bin/bash
RUN su ansible -c "ssh-keygen -t ecdsa -f /home/ansible/.ssh/ansible_ecdsa -N ''"
RUN su ansible -c 'cat /home/ansible/.ssh/ansible_ecdsa.pub >> /home/ansible/.ssh/authorized_keys'

# ansibleを実行しない一般ユーザー
RUN useradd -m app -s /bin/bash

CMD ["/usr/sbin/sshd", "-D"]

## rootユーザーから実行して、もう一つシェルを開き、そこから実行。
# /usr/sbin/sshd -D
# su ansible
# ssh ansible@localhost -i ~/.ssh/ansible_ecdsa
# ansible-playbook -i /workspaces/ansible/inventory.ini /workspaces/ansible/playbook.yml --private-key=/home/ansible/.ssh/ansible_ecdsa
