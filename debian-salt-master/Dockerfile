FROM badele/debian-mybase
MAINTAINER Bruno Adelé "bruno@adele.im"


# Add salt repository & Upgrade the distribution
RUN echo "deb http://debian.saltstack.com/debian jessie-saltstack main" >> /etc/apt/sources.list
RUN wget -q -O- "http://debian.saltstack.com/debian-salt-team-joehealy.gpg.key" | apt-key add -
RUN apt-get update
RUN apt-get -yf upgrade
RUN apt-get -yf dist-upgrade

# Install salt master
RUN apt-get install -y salt-master


# Clean the cache and unused packages
RUN apt-get clean
RUN apt-get autoremove

# Volume can be backuped
VOLUME ["/etc/salt" "/var/cache/salt" "/srv/salt]

CMD bash
