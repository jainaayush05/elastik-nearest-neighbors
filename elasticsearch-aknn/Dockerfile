FROM docker.elastic.co/elasticsearch/elasticsearch:6.2.4

ADD . /aknn
WORKDIR /aknn


# Install Java 10
# Before building this image, download the .rpm files to elasticsearch-aknn directory
#     from http://www.oracle.com/technetwork/java/javase/downloads/index.html

#https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase10-4425482.html

RUN yum -y install jdk-10.0.2_linux-x64_bin.rpm
RUN yum -y install jre-10.0.2_linux-x64_bin.rpm
ENV JAVA_HOME=/usr/java/jdk-10.0.2/

wget --no-cookies --no-check-certificate --header "Cookie: RT=sl=16&ss=1544305611360&tt=105022&obo=1&bcn=%2F%2F36fb68c2.akstat.io%2F&sh=1544307107821%3D16%3A1%3A105022%2C1544306418289%3D15%3A1%3A96370%2C1544306220860%3D14%3A1%3A89660%2C1544306204378%3D13%3A1%3A85180%2C1544306192178%3D12%3A1%3A80502&dm=oracle.com&si=2fc3a2b6-451e-42e3-a702-0d0d43f6bed0&ld=1544307107821&nu=http%3A%2F%2Fdownload.oracle.com%2Fotn%2Fjava%2Fjdk%2F10.0.2%2B13%2F19aef61b38124481863b1413dce1855f%2Fjdk-10.0.2_linux-x64_bin.rpm&cl=1544307254064;gpw_e24=https%3A%2F%2Fwww.oracle.com%2Ftechnetwork%2Fjava%2Fjavase%2Fdownloads%2Fjava-archive-javase10-4425482.html; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn/java/jdk/10.0.2+13/19aef61b38124481863b1413dce1855f/jdk-10.0.2_linux-x64_bin.rpm"


# Install gradle 4.9

RUN wget https://services.gradle.org/distributions/gradle-4.9-bin.zip
RUN mkdir /opt/gradle
RUN unzip -d /opt/gradle gradle-4.9-bin.zip
ENV PATH=$PATH:/opt/gradle/gradle-4.9/bin


# Build & install the plugin

RUN gradle clean build -x integTestRunner -x test
RUN elasticsearch-plugin install -b file:build/distributions/elasticsearch-aknn-0.0.1-SNAPSHOT.zip

# Configure ElasticSearch
ENV ES_JAVA_OPTS="-Xms10g -Xmx10g"
