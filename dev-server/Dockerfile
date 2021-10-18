FROM ubuntu:latest
MAINTAINER pwnlxrd♥suslik
ARG SSH_USER=user
ARG SSH_USER_GROUP=user
ARG SSH_USER_ID=1000
ARG SSH_USER_GROUP_ID=1000

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends openssh-server sudo vim git git-flow bash-completion golang python3 && \
    apt-get autoremove -y && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/* && \
    \
    mkdir -p /var/run/sshd && \
    \
    sed 's/#PasswordAuthentication yes/PasswordAuthentication yes/' -i /etc/ssh/sshd_config && \
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd && \
    sed 's/^%sudo.*/%sudo\tALL=NOPASSWD: ALL/' -i /etc/sudoers && \
    { \
    echo "export VISIBLE=now"; \
    echo "export GOPATH=/go"; \
    echo "export PATH=/go/bin:/usr/local/go/bin:"\$"{PATH}"; \
    } | tee -a /etc/profile && \
    \
    groupadd -g ${SSH_USER_GROUP_ID} ${SSH_USER_GROUP} && \
    useradd -u ${SSH_USER_ID} -g ${SSH_USER_GROUP_ID} -G sudo -s /bin/bash -m ${SSH_USER} && \
    \
    { \
    echo 'mkdir ~/.ssh && chmod 700 ~/.ssh'; \
    echo 'touch ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys'; \
    echo 'mkdir ~/.vscode-server'; \
    } | sudo -u ${SSH_USER} /bin/bash
ENV NOTVISIBLE "in users profile"
ENV EDITOR=vim
EXPOSE 22
WORKDIR /data
ENTRYPOINT ["sudo", "/usr/sbin/sshd", "-D"]

