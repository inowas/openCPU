# Use builds from launchpad
FROM ubuntu:20.04

ENV DEBIAN_FRONTEND noninteractive

RUN R --version

# Install R4.0 manually
RUN \
  apt-get update && \
  apt-get -y dist-upgrade && \
  apt-get install -y dirmngr gnupg apt-transport-https ca-certificates software-properties-common && \
  apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 && \
  add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/'

# Install and 'hold' opencpu-server so that the docker image can be tagged
RUN \
  apt-get update && \
  apt-get -y dist-upgrade && \
  apt-get install -y software-properties-common && \
  add-apt-repository -y ppa:opencpu/opencpu-2.2 && \
  apt-get update && \
  apt-get install -y opencpu-server && \
  apt-mark hold opencpu-server

# Prints apache logs to stdout
RUN \
  ln -sf /proc/self/fd/1 /var/log/apache2/access.log && \
  ln -sf /proc/self/fd/1 /var/log/apache2/error.log && \
  ln -sf /proc/self/fd/1 /var/log/opencpu/apache_access.log && \
  ln -sf /proc/self/fd/1 /var/log/opencpu/apache_error.log

# Set opencpu password so that we can login
RUN \
  echo "opencpu:opencpu" | chpasswd

# Apache ports
EXPOSE 80
EXPOSE 443
EXPOSE 8004

# Start non-daemonized webserver
CMD service cron start && apachectl -DFOREGROUND


# Now install the relevent packages
RUN R -e 'install.packages("remotes", repos = "https://cloud.r-project.org")'
RUN R -e 'remotes::install_github("KWB-R/kwb.heatsine.opencpu@v0.1.0")'
RUN R -e 'remotes::install_github("KWB-R/kwb.qmra@e23bd9f7ec05b43fb9eb92456f5fafff01b70ab7")'
