FROM amazonlinux:latest
MAINTAINER Ashish.kasaudhan

ARG VERSION=3.28
ARG USER=jenkins
ARG GROUP=jenkins
ARG UID=1000
ARG GID=1000
ARG AGENT_WORKDIR=/home/${USER}/agent
ENV HOME /home/${USER}
ENV AGENT_WORKDIR=${AGENT_WORKDIR}
ENV JAVA_HOME /usr/lib/jvm/java 

RUN amazon-linux-extras enable corretto8 && \
  yum clean metadata && \
  yum -y update && \
  yum -y install java-1.8.0-amazon-corretto-devel shadow-utils.x86_64 && \
  yum clean all && \
  rm -rf /var/cache/yum

RUN groupadd -g ${GID} ${GROUP}

RUN useradd -c "Jenkins user" -d $HOME -u ${UID} -g ${GID} -m ${USER}

RUN curl --create-dirs -fsSLo /usr/share/jenkins/slave.jar https://repo.jenkins-ci.org/public/org/jenkins-ci/main/remoting/${VERSION}/remoting-${VERSION}.jar \
  && chmod 755 /usr/share/jenkins \
  && chmod 644 /usr/share/jenkins/slave.jar

RUN yum update -y
RUN yum install python-pip unzip wget git  -y && \
    pip install ansible && \ 
    pip install awscli && \ 
    wget https://releases.hashicorp.com/packer/1.3.4/packer_1.3.4_linux_amd64.zip && \
    unzip packer_1.3.4_linux_amd64.zip  && \
    mv packer /usr/local/bin/ && \
    packer version && \ 
    ansible --version && \
    git version && \
    wget https://packages.chef.io/files/stable/inspec/4.18.85/el/7/inspec-4.18.85-1.el7.x86_64.rpm && \
    rpm -ivh inspec-4.18.85-1.el7.x86_64.rpm && \
    inspec version

USER ${USER}

RUN mkdir /home/${USER}/.jenkins && mkdir -p ${AGENT_WORKDIR}

VOLUME /home/${USER}/.jenkins

VOLUME ${AGENT_WORKDIR}

WORKDIR /home/${USER}





