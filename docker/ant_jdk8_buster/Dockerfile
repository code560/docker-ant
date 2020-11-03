FROM buildpack-deps:buster-scm

ARG JDK_VERSION=8
ARG JDK_UPDATE=121
ARG JDK_DISTRO_ARCH=linux-x64
ENV JDK_FOLDER=jdk1.${JDK_VERSION}.0_${JDK_UPDATE}
ENV JDK_NAME=jdk-${JDK_VERSION}'u'${JDK_UPDATE}-${JDK_DISTRO_ARCH}
ENV JAVA_HOME=/opt/java

ARG ANT_VERSION=1.10.3
ENV ANT_NAME=apache-ant-${ANT_VERSION}-bin
ENV ANT_HOME=/opt/ant

RUN set -ex ;\
    apt-get update ;\
    apt-get install -y --no-install-recommends \
    dpkg-dev \
    patch \
    unzip ;\
    apt-get clean ;\
    rm -rf /var/lib/apt/lists/*

WORKDIR /tmp

COPY ./jdk/${JDK_NAME}.tar.gz /tmp

RUN tar -zvxf ${JDK_NAME}.tar.gz -C /opt ;\
    ln -s /opt/${JDK_FOLDER} /opt/java ;\
    rm -f ${JDK_NAME}.tar.gz ;\
    update-alternatives --install "/usr/bin/java" "java" "/opt/java/bin/java" 1 ;\
    update-alternatives --set "java" "/opt/java/bin/java" ;\
    update-alternatives --install "/usr/bin/javac" "javac" "/opt/java/bin/javac" 1 ;\
    update-alternatives --set "javac" "/opt/java/bin/javac" ;\
    update-alternatives --install "/usr/bin/keytool" "keytool" "/opt/java/bin/keytool" 1 ;\
    update-alternatives --set "keytool" "/opt/java/bin/keytool"

RUN wget --no-check-certificate --no-cookies https://archive.apache.org/dist/ant/binaries/${ANT_NAME}.tar.gz ;\
    wget --no-check-certificate --no-cookies https://archive.apache.org/dist/ant/binaries/${ANT_NAME}.tar.gz.sha512 ;\
    echo "$(cat ${ANT_NAME}.tar.gz.sha512) ${ANT_NAME}.tar.gz" | sha512sum -c ;\
    tar -zvxf ${ANT_NAME}.tar.gz -C /opt/ ;\
    ln -s /opt/apache-ant-${ANT_VERSION} /opt/ant ;\
    rm -f ${ANT_NAME}.tar.gz ;\
    rm -f ${ANT_NAME}.tar.gz.sha512 ;\
    update-alternatives --install "/usr/bin/ant" "ant" "/opt/ant/bin/ant" 1 ;\
    update-alternatives --set "ant" "/opt/ant/bin/ant"

WORKDIR /root
