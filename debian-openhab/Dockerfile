FROM badele/debian-openjdk:7
MAINTAINER Bruno Adelé "bruno@adele.im"

USER root
ENV DEBIAN_FRONTEND noninteractive
ENV DISPLAY :0

# Install requirements
RUN apt-get update
RUN apt-get install -y curl unzip

# Download openhab packages
RUN curl --create-dirs -L https://bintray.com/artifact/download/openhab/bin/distribution-1.7.0-designer-linux64bit.zip -o /opt/openhab-designer/distribution-1.7.0-designer-linux64bit.zip
RUN curl --create-dirs -L https://bintray.com/artifact/download/openhab/bin/distribution-1.7.0-runtime.zip -o /opt/openhab/distribution-1.7.0-runtime.zip
RUN curl --create-dirs -L https://bintray.com/artifact/download/openhab/bin/distribution-1.7.0-addons.zip -o /opt/openhab/addons_src/distribution-1.7.0-addons.zip

# Install requirement for domolib
RUN apt-get install -y git python-dev python2.7 python-pip

# Install domolib
RUN pip install git+git://github.com/badele/domolib.git

# Install openhab designer
WORKDIR /opt/openhab-designer
RUN unzip distribution-1.7.0-designer-linux64bit.zip
RUN rm distribution-1.7.0-designer-linux64bit.zip

# Install runtime
WORKDIR /opt/openhab
RUN unzip distribution-1.7.0-runtime.zip
RUN rm distribution-1.7.0-runtime.zip

# Install openhab web designer
RUN curl -LO https://github.com/cdjackson/HABmin/releases/download/0.1.3-snapshot/habmin.zip
RUN unzip habmin.zip

# Configure openHAB
ADD files/configurations/ configurations/
#RUN mv configurations/sitemaps/demo.sitemap configurations/sitemaps/default.sitemap
RUN chmod +x start.sh

# Install temporary addons
WORKDIR /opt/openhab/addons_src
RUN unzip distribution-1.7.0-addons.zip
RUN rm distribution-1.7.0-addons.zip

# Move addons
RUN mv org.openhab.binding.http* ../addons/
RUN mv org.openhab.binding.ntp* ../addons/
RUN mv org.openhab.binding.exec* ../addons/
RUN mv org.openhab.*mqtt* ../addons/
RUN mv org.openhab.persistence.rrd4j* ../addons/
RUN mv org.openhab.persistence.db4o* ../addons/
RUN mv org.openhab.persistence.logging* ../addons/


# Install my demo
WORKDIR /opt/openhab
ADD files/items/ configurations/items/
ADD files/persistence/ configurations/persistence/
ADD files/rules/ configurations/rules/
ADD files/scripts/ configurations/scripts/
ADD files/sitemaps/ configurations/sitemaps/
ADD files/transform/ configurations/transform/
ADD files/configurations/ configurations/

VOLUME [ /opt/openhab ]
EXPOSE 8080 8443 5555 9001

# Launch the server
CMD /opt/openhab/start.sh