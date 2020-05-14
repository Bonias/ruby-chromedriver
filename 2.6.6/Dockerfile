FROM ruby:2.6.6

MAINTAINER Piotr Boniecki <piotr@prograils.com>

ENV DEBIAN_FRONTEND noninteractive
ENV CHROME_VERSION 81.0.4044.92-1
ENV CHROMEDRIVER_VERSION 81.0.4044.69

# Install usefull packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    # required to install ruby-filemagic gem
    libmagic-dev \
    ghostscript \
    xvfb \
 && rm -rf /var/lib/apt/lists/*

# Install yarn (this also installs nodejs)
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
 && echo "deb https://dl.yarnpkg.com/debian/ stable main" >> /etc/apt/sources.list.d/yarn.list \
 && apt-get update && apt-get -y install yarn \
 && rm -rf /var/lib/apt/lists/*

# Install Chrome
RUN curl -sS https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
 && echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list \
 && apt-get update && apt-get -y --no-install-recommends install google-chrome-stable=$CHROME_VERSION \
 && rm -rf /var/lib/apt/lists/*

# Install Chromedriver
RUN wget -O /tmp/chromedriver.zip http://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip \
 && unzip /tmp/chromedriver.zip chromedriver -d /usr/bin/ \
 && rm /tmp/chromedriver.zip \
 && chmod ugo+rx /usr/bin/chromedriver \
 && apt-mark hold google-chrome-stable

# install libjpeg, libssl1.0.0 and libpng12 required to run wkhtmltopdf provided by wkhtmltopdf-binary-edge
RUN apt-get update && apt-get -y --no-install-recommends install multiarch-support && rm -rf /var/lib/apt/lists/* \
 && wget -O libjpeg.deb http://archive.ubuntu.com/ubuntu/pool/main/libj/libjpeg-turbo/libjpeg-turbo8_1.4.2-0ubuntu3.3_amd64.deb \
 && dpkg -i libjpeg.deb && rm libjpeg.deb \
 && wget -O libpng.deb http://security.ubuntu.com/ubuntu/pool/main/libp/libpng/libpng12-0_1.2.54-1ubuntu1.1_amd64.deb \
 && dpkg -i libpng.deb && rm libpng.deb \
 && wget -O libssl.deb http://security.debian.org/debian-security/pool/updates/main/o/openssl/libssl1.0.0_1.0.1t-1+deb8u12_amd64.deb \
 && dpkg -i libssl.deb && rm libssl.deb
