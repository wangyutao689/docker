##docker
1、基于alpine制作jdk8镜像
准备好locale.md文件，解决日志中文乱码的问题。文件内容只需要列出我们需要的编码即可
en_US
zh_CN
zh_HK
zh_SG
zh_TW
zu_ZA

2、优化java 包
解压tar包
tar zxvf jdk-8u211-linux-x64.tar.gz

删除无用文件
cd jdk1.8.0_211/
rm -rf COPYRIGHT LICENSE README release THIRDPARTYLICENSEREADME-JAVAFX.txt THIRDPARTYLICENSEREADME.txt Welcome.html
rm -rf   lib/plugin.jar \
           lib/ext/jfxrt.jar \
           bin/javaws \
           lib/javaws.jar \
           lib/desktop \
           plugin \
           lib/deploy* \
           lib/*javafx* \
           lib/*jfx* \
           lib/amd64/libdecora_sse.so \
           lib/amd64/libprism_*.so \
           lib/amd64/libfxplugins.so \
           lib/amd64/libglass.so \
           lib/amd64/libgstreamer-lite.so \
           lib/amd64/libjavafx*.so \
           lib/amd64/libjfx*.so

重新打包
tar acf java1.8.tar.gz jdk1.8.0_211

3. Dockerfile文件内容

glibc-2.30-r0.apk glibc-bin-2.30-r0.apk glibc-i18n-2.30-r0.apk 要提前下载好，避免构建下载失败启动报错

下载地址：

https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.30-r0/glibc-2.30-r0.apk 

https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.30-r0/glibc-bin-2.30-r0.apk 

https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.30-r0/glibc-i18n-2.30-r0.apk
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


2. 执行docker build指令
docker build --rm -t alpine-java .

