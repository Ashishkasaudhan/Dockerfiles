FROM amazonlinux:latest
MAINTAINER Ashish.kasaudhan
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
