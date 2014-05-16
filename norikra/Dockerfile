FROM base

MAINTAINER ixixi "https://github.com/ixixi"

# Install packages for building ruby,jruby
RUN apt-get update
RUN apt-get install -y --force-yes build-essential curl git
RUN apt-get install -y --force-yes zlib1g-dev libssl-dev libreadline-dev libyaml-dev libxml2-dev libxslt-dev
RUN apt-get install -y openjdk-7-jre-headless
RUN apt-get clean
ENV JAVA_HOME /usr/lib/jvm/java-7-openjdk-amd64

# Install rbenv and ruby-build
RUN git clone https://github.com/sstephenson/rbenv.git /root/.rbenv
RUN git clone https://github.com/sstephenson/ruby-build.git /root/.rbenv/plugins/ruby-build
RUN ./root/.rbenv/plugins/ruby-build/install.sh
ENV PATH /root/.rbenv/bin:$PATH
RUN echo 'eval "$(rbenv init -)"' >> /etc/profile.d/rbenv.sh # or /etc/profile
RUN echo 'eval "$(rbenv init -)"' >> .bashrc
ENV CONFIGURE_OPTS --disable-install-doc

# install rubies
RUN rbenv install 2.1.1
RUN rbenv install jruby-1.7.9

# Install Bundler for each version of ruby
RUN echo 'gem: --no-rdoc --no-ri' >> /.gemrc
RUN bash -l -c 'for v in $(echo "jruby-1.7.9 2.1.1"); do rbenv global $v; gem install bundler; done'

# Install norikra
RUN bash -l -c 'rbenv global jruby-1.7.9 ; gem install norikra'
RUN mkdir /var/log/norikra

# Install fluentd, fluent-plugin-norikra
RUN bash -l -c 'rbenv shell 2.1.1 ; gem install fluentd fluent-plugin-norikra'
RUN mkdir ~/fluentd
RUN echo "2.1.1" > ~/fluentd/.ruby-version
ADD fluentd_ex1.conf ~/fluentd/
ADD fluentd_ex2.conf ~/fluentd/
