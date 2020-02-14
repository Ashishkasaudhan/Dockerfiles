FROM jenkins/ssh-slave

USER root

# Enable Ansible Pipelining & Mitogen Plugin for Faster Builds
ENV ANSIBLE_STRATEGY=mitogen_linear \
    ANSIBLE_STRATEGY_PLUGINS=/opt/ansible/mitogen-0.2.9/ansible_mitogen/plugins/strategy \
    ANSIBLE_PIPELINING=1

RUN apt-get update \
    && apt-get -y --no-install-recommends install \
    python-pip awscli unzip wget git  default-jdk && \
    pip install setuptools && \
    java -version 
# install Packer
RUN set -ex && cd ~ \
  && curl -LO https://releases.hashicorp.com/packer/1.4.3/packer_1.4.3_linux_amd64.zip \
  && [ $(sha256sum packer_1.4.3_linux_amd64.zip | cut -f1 -d ' ') = c89367c7ccb50ca3fa10129bbbe89273fba0fa6a75b44e07692a32f92b1cbf55 ] \
  && unzip -d /usr/local/bin packer_1.4.3_linux_amd64.zip \
  && rm -vf packer_1.4.3_linux_amd64.zip

# install inspec
RUN set -ex && cd ~ \
  && curl -LO https://packages.chef.io/files/stable/inspec/3.9.3/ubuntu/18.04/inspec_3.9.3-1_amd64.deb \
  && [ $(sha256sum inspec_3.9.3-1_amd64.deb | cut -f1 -d ' ') = 757dd2366a3932adc5fcc9382b30d77de6cc33152585f4c9f94f8918d9d349a7 ] \
  && dpkg -i inspec_3.9.3-1_amd64.deb \
  && /opt/inspec/embedded/bin/gem install rspec-retry \
  && rm -vf inspec_3.9.3-1_amd64.deb

# install Python packages
ARG CACHE_PIP_PACKER
ADD ./requirements.txt /tmp/requirements.txt
RUN set -ex && cd ~ \
  && pip install -r /tmp/requirements.txt --no-cache-dir --disable-pip-version-check \
  && rm -vf /tmp/requirements.txt \
  && wget https://networkgenomics.com/try/mitogen-0.2.9.tar.gz -O /tmp/mitogen-0.2.9.tar.gz \
  && tar -xvzf /tmp/mitogen-0.2.9.tar.gz -C /tmp \
  && mkdir -p /opt/ansible \
  && mv /tmp/mitogen-0.2.9 /opt/ansible/mitogen-0.2.9 \
  && rm -rf /tmp/*

RUN ansible --version \
    && packer version \
    && inspec version
