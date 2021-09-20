FROM amazoncorretto:11.0.12
ARG TOMCAT_VERSION=9.0.53
ARG TOMCAT_TAR_URL=https://dlcdn.apache.org/tomcat/tomcat-9/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz
ARG USER_NAME=tomcat
ARG GROUP_NAME=tomcat
ARG UID=1000
ARG GID=1000
ENV CATALINA_HOME=/opt/tomcat
# Tomcatインストールのためのツール
RUN yum install -y tar gzip shadow-utils.x86_64 supervisor && \
    yum clean all && \
    rm -rf /var/cache/yum
# Tomcatユーザを作成
RUN groupadd -g ${GID} ${GROUP_NAME} && \
    useradd -m -s /bin/bash -u ${UID} -g ${GID} ${USER_NAME}
RUN mkdir -p ${CATALINA_HOME} && \
    chown ${UID}:${GID} ${CATALINA_HOME}
USER ${USER_NAME}
ENV WORK_DIR=/home/${USER_NAME}/tmp
WORKDIR ${WORK_DIR}
# Tomcatをインストール
RUN curl -O ${TOMCAT_TAR_URL} && \
    curl "${TOMCAT_TAR_URL}.sha512" | sha512sum -c - && \
    tar zxvf ${TOMCAT_TAR_URL##*/} -C ${CATALINA_HOME} --strip-components 1 && \
    chmod +x ${CATALINA_HOME}/bin/catalina.sh && \
    rm -rf ${WORK_DIR}
# 起動時のエントリーポイント設定
# CMD [ "run" ]
# ENTRYPOINT [ "/bin/bash", "-c", "${CATALINA_HOME}/bin/catalina.sh ${@}", "--" ]
# ENTRYPOINT [ "/bin/bash", "-c", "source ~/.bashrc && ${CATALINA_HOME}/bin/catalina.sh ${@}", "--" ]

# supervisordで管理
USER root
RUN amazon-linux-extras install -y epel && \
    yum install -y supervisor && \
    yum clean all && \
    rm -rf /var/cache/yum
ADD supervisord.conf /etc/supervisor/conf.d/
ENTRYPOINT [ "/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
