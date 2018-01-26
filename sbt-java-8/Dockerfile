FROM centos/s2i-base-centos7

MAINTAINER Alastair Munro

ENV SBT_BUILDS ""
ENV BUILD "/opt/build"

LABEL io.k8s.description="Build and run Scala applications Java 8" \
      io.k8s.display-name="Scala with Java 8" \
      io.openshift.tags="builder,scala,sbt" \
      io.openshift.min-memory="512Mi"

RUN curl -ns https://bintray.com/sbt/rpm/rpm > /etc/yum.repos.d/bintray-sbt-rpm.repo && \
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo && \
    yum update -y && \
    yum install -y java-1.8.0-openjdk-devel sbt && \
    mkdir $BUILD && \
    chown default $BUILD && \
    yum clean all -y && \
    rm -rf /var/cache/yum

COPY s2i/bin/ $STI_SCRIPTS_PATH

USER 1001

CMD $STI_SCRIPTS_PATH/usage
