FROM centos/s2i-base-centos7

MAINTAINER Alastair Munro

ENV SBT_BUILDs ""

LABEL io.k8s.description="Build and run Scala applications Java 8" \
      io.k8s.display-name="Scala with Java 8" \
      io.openshift.tags="builder,scala,sbt" \
      io.openshift.min-memory="512Mi"

ADD bintray-sbt-rpm.repo /etc/yum.repos.d/bintray-sbt-rpm.repo

RUN yum update -y && \
    yum install -y java-1.8.0-openjdk-devel && \
    curl https://bintray.com/sbt/rpm/rpm | tee /etc/yum.repos.d/bintray-sbt-rpm.repo && \
    yum install -y sbt && \
    set -o pipefail && curl -L https://github.com/openshift/origin/releases/download/v3.6.0/openshift-origin-client-tools-v3.6.0-c4dd4cf-linux-64bit.tar.gz | \
    tar -zx && \
    mv openshift*/oc /usr/local/bin && \
    rm -rf openshift-origin-client-tools-* && \
    yum clean all -y && \
    rm -rf /var/cache/yum

RUN sbt -ivy /opt/app-root/src/.ivy2 about
RUN chown -R 1001:0 /opt/app-root/src/.ivy2; chmod -R g+rw /opt/app-root/src/.ivy2

COPY s2i/bin/ $STI_SCRIPTS_PATH

USER 1001

CMD $STI_SCRIPTS_PATH/usage
