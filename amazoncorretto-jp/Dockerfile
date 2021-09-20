FROM amazoncorretto:11.0.12
# 言語設定
RUN yum update -y && \
    yum install -y glibc-locale-source glibc-langpack-en glibc-langpack-ja && \
    yum install -y gzip && \
    yum clean all && \
    rm -rf /var/cache/yum
RUN localedef --charmap UTF-8 --inputfile ja_JP ja_JP.UTF-8
# タイムゾーン設定
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
ENV LANG="ja_JP.UTF-8" \
    LANGUAGE="ja_JP:ja" \
    LC_ALL="ja_JP.UTF-8" \
    TZ="Asia/Tokyo"