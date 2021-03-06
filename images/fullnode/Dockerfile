############################################################
# Dockerfile to build resolver
# Based on Ubuntu
############################################################

# Set the base image to Ubuntu
FROM digitalpassport/namecoind

# File Author / Maintainer
MAINTAINER Muneeb Ali (@muneeb)

################## BEGIN INSTALLATION ######################

RUN apt-get update 

# Install Apache2
RUN apt-get install -y \
    apache2 \
    apache2-utils \
    libapache2-mod-wsgi \
    git \
    python-pip

RUN a2enmod headers

# Install Memcached
RUN apt-get install -y python-dev \
    libmemcached-dev \
    zlib1g-dev \
    memcached

# Create the default directory (uncomment if doesn't exist)
#RUN mkdir -p /srv

# Clone the app from github and install packages
RUN git clone https://github.com/openname/resolver.git /srv/resolver

# Uncomment below if using a local tar.gz file
#ADD resolver.tar.gz /srv/

WORKDIR /srv/resolver
RUN pip install -r requirements.txt

# Copy the Apache config files
RUN cp -f /srv/resolver/apache/config/default.conf /etc/apache2/sites-available/000-default.conf
RUN cp -f /srv/resolver/apache/config/wsgi.conf /etc/apache2/conf-enabled

RUN cp -f /srv/resolver/image/fullnode/run.sh /srv/resolver/
RUN cp -f /srv/resolver/image/fullnode/config.py /srv/resolver/server/config_local.py
RUN chmod 755 /srv/resolver/run.sh

##################### INSTALLATION END #####################

# Expose the default port
EXPOSE 80

# Set default entry point
ENTRYPOINT /srv/resolver/run.sh