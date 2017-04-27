FROM centos:7
MAINTAINER Julius Loman <lomo@kyberia.net>
# Forked from: https://github.com/juliusloman/docker-omnibus
# Updated with Latest Netcool/Omnibus Core 8.1 Fixpack 11

# Update with your local http server for installation binary download
ENV INSTALLHOST="http://172.17.0.1:8080" 

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
COPY webgui-response.xml run_jazzsm.sh /tmp/install/scripts/

# Add dedicated user
RUN mkdir -p /opt/IBM /tmp/install/was /tmp/install/jazz /tmp/install/webgui /tmp/install/webgui-fp /tmp/install/im && \
    useradd -c "Netcool user" netcool && mkdir -p /db /tmp/install && chown -R netcool:netcool /db /tmp/install /opt/IBM && \
    mv /tmp/install/scripts/run_jazzsm.sh /

USER netcool

# Perform installation
RUN cd /tmp/install && \
    curl -O $INSTALLHOST/agent.installer.linux.gtk.x86_64_1.8.6000.20161118_1611.zip && unzip -q -d /tmp/install/im agent.installer.linux.gtk.x86_64_1.8.6000.20161118_1611.zip && \
    curl -O $INSTALLHOST/WAS_V8.5.5.9_FOR_JSM_FOR_LINUX_ML.zip && unzip -q -d /tmp/install/was WAS_V8.5.5.9_FOR_JSM_FOR_LINUX_ML.zip && \
    curl -O $INSTALLHOST/JAZZ_FOR_SM_1.1.3.0_FOR_LNX.zip && unzip -q -d /tmp/install/jazz JAZZ_FOR_SM_1.1.3.0_FOR_LNX.zip && \
    curl -O $INSTALLHOST/OMNIbus-v8.1.0.4-WebGUI.Linux64.zip && unzip -q -d /tmp/install/webgui OMNIbus-v8.1.0.4-WebGUI.Linux64.zip && \
    curl -O $INSTALLHOST/OMNIbus-v8.1.0-WebGUI-FP9-IM-Extensions-linux64-UpdatePack.zip && unzip -q -d /tmp/install/webgui-fp OMNIbus-v8.1.0-WebGUI-FP9-IM-Extensions-linux64-UpdatePack.zip && \
    /tmp/install/im/userinstc --launcher.ini /tmp/install/im/user-silent-install.ini  -acceptLicense && \
    /home/netcool/IBM/InstallationManager/eclipse/IBMIM -acceptLicense -ShowVerboseProgress -silent -nosplash -input /tmp/install/scripts/webgui-response.xml && \
    /opt/IBM/JazzSM/profile/bin/startServer.sh server1 -script /opt/IBM/JazzSM/profile/bin/startJazz.sh && \
    chmod +x /run_jazzsm.sh && \ 
    rm -rf /tmp/install

CMD /run_jazzsm.sh
