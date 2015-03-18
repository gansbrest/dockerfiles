# Version: 0.0.1
FROM ubuntu:14.04

# Install.
RUN \
  sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list && \
  apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y build-essential zlib1g-dev libssl-dev libreadline6-dev libyaml-dev && \
  apt-get install -y software-properties-common && \
  apt-get install -y byobu curl git htop man unzip vim wget

# Install Ruby 2.1.2
RUN \
  cd /tmp && \
  wget http://ftp.ruby-lang.org/pub/ruby/2.1/ruby-2.1.2.tar.gz && \
  tar -xvzf ruby-2.1.2.tar.gz && \
  cd ruby-2.1.2/ && \
  ./configure --prefix=/usr/local && \
  make && \
  make install && \
  cd /tmp && \
  rm -rf ruby-*

# Set environment variables.
ENV HOME /root

# Define mountable directories.
VOLUME ["/root"]

# Configure locales
RUN locale-gen en_US.UTF-8  
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8
