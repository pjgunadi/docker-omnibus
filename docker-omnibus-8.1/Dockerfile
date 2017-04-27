FROM centos:latest
MAINTAINER Julius Loman <lomo@kyberia.net>
# Forked from: https://github.com/juliusloman/docker-omnibus
# Updated with Latest Netcool/Omnibus Core 8.1 Fixpack 11

# Update with your local http server for installation binary download
ENV INSTALLHOST="http://172.17.0.1:8080" NCHOME="/opt/IBM/tivoli/netcool" OMNIHOME="/opt/IBM/tivoli/netcool/omnibus" OBJSRV="NCOMS" OBJSRV_PORT=4100 

# Add dependencies
RUN yum --setopt=tsflags=nodocs -y install \
    tar \
    hostname \
    unzip \
    audit-libs \
    fontconfig \
    freetype \
    compat-libstdc++-33.i686 \
    glibc.i686 \
    gtk2 \
    libICE \
    libSM \
    libX11 \
    libXau \
    libXcursor \
    libXext \
    libXft \
    libXmu \
    libXp \
    libXpm \
    libXrender \
    libXt \
    libXtst \
    libstdc++.i686 \
    libgcc.i686 \
    libjpeg-turbo \
    libpng12 \
    motif \
    dejavu-fonts-common \
    dejavu-sans-fonts \
    expat \
    glibc \
    libgcc \
    libidn \
    libstdc++ \
    libuuid \
    libxcb \
    nss-softokn-freebl \
    pam \
    zlib \
    nss-softokn-freebl.i686 \
    compat-libstdc++-33 && \
    rm -rf /var/cache/yum/* && \
    yum clean all

# Add installation media
COPY run_objectserver.sh run_nco.sh omnibus-response.xml /tmp/install/scripts/

# Add dedicated user
RUN mkdir -p $NCHOME /tmp/install/core /tmp/install/fp /tmp/install/im && \
    useradd -c "Netcool user" netcool && mkdir -p /db /tmp/install && chown -R netcool:netcool /db /tmp/install $NCHOME && \
    mv /tmp/install/scripts/run_objectserver.sh /tmp/install/scripts/run_nco.sh /

USER netcool

# Perform installation
RUN cd /tmp/install && \
    curl -O $INSTALLHOST/OMNIbus-v8.1.0.5-Core.linux64.zip &&  unzip -q -d /tmp/install/core OMNIbus-v8.1.0.5-Core.linux64.zip && \
    curl -O $INSTALLHOST/8.1.0-TIV-NCOMNIbus-Linux-FP0011.zip && unzip -q -d /tmp/install/fp 8.1.0-TIV-NCOMNIbus-Linux-FP0011.zip && \
    curl -O $INSTALLHOST/agent.installer.linux.gtk.x86_64_1.8.6000.20161118_1611.zip && unzip -q -d /tmp/install/im agent.installer.linux.gtk.x86_64_1.8.6000.20161118_1611.zip && \
    /tmp/install/im/userinstc --launcher.ini /tmp/install/im/user-silent-install.ini  -acceptLicense && \
    /home/netcool/IBM/InstallationManager/eclipse/IBMIM -acceptLicense -ShowVerboseProgress -silent -nosplash -input /tmp/install/scripts/omnibus-response.xml && \
    chmod +x /run_objectserver.sh && \ 
# Optially remove Installation Manager stuff to lower image footprint
#   rm -rf /home/netcool/var /home/netcool/IBM && \
    rm -rf /tmp/install

VOLUME /db

CMD /run_objectserver.sh
