# Version: 0.0.1
FROM gansbrest/ubuntu-base

# Install Node.js
RUN \
  cd /tmp && \
  wget http://nodejs.org/dist/v0.10.30/node-v0.10.30-linux-x64.tar.gz && \
  tar xvzf node-v0.10.30-linux-x64.tar.gz && \
  rm -f node-v0.10.30-linux-x64.tar.gz && \
  cd node-v* && \
  cp -r * /usr && \
  cd /tmp && \
  rm -rf /tmp/node-v*

# Install other deps
RUN \
  npm install -g nodemon && \
  npm install -g gulp && \
  npm install -g clean-css && \
  gem install compass -v 1.0.0.alpha.20 --no-ri --no-rdoc


EXPOSE 8001
