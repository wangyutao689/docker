#******************Alpine安装 Glibc https://github.com/sgerrand/alpine-pkg-glibc *****************
FROM alpine:latest
 
#******************Alpine安装 Glibc https://github.com/sgerrand/alpine-pkg-glibc *****************
RUN echo "https://mirrors.aliyun.com/alpine/v3.9/main/" > /etc/apk/repositories && \
wget http://10.0.120.245/java/locale.md && \
wget http://10.0.120.245/java/java1.8.tar.gz && \
mv java1.8.tar.gz /usr/local/ && \
cd /usr/local/ && \
tar -xf java1.8.tar.gz && \
rm -rf java1.8.tar.gz && \
apk --no-cache add ca-certificates wget && \
wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub && \
wget http://10.0.120.245/java/glibc-2.30-r0.apk && \
wget http://10.0.120.245/java/glibc-bin-2.30-r0.apk && \
wget http://10.0.120.245/java/glibc-i18n-2.30-r0.apk && \
apk add glibc-bin-2.30-r0.apk glibc-i18n-2.30-r0.apk glibc-2.30-r0.apk && \
cat /locale.md | xargs -i /usr/glibc-compat/bin/localedef -i {} -f UTF-8 {}.UTF-8 && \
apk add curl && \
rm -rf *.apk && \
rm -rf /var/cache/apk/* &&\
rm -rf /locale.md
 
#******************设置JAVA变量环境******************
ENV JAVA_HOME=/usr/local/jdk1.8.0_211/jre
ENV CLASSPATH=$JAVA_HOME/bin
ENV PATH=.:$JAVA_HOME/bin:$PATH

#******************指定编码******************
ENV LANG=zh_CN.UTF-8 \
  LANGUAGE=zh_CN.UTF-8