FROM centos/s2i-base-centos7

MAINTAINER Alastair Munro

ENV SBT_BUILDs ""
ENV OC_VER "v3.6.0"
ENV OC_VER_COMMIT "c4dd4cf"

LABEL io.k8s.description="Build and run Scala applications Java 8" \
      io.k8s.display-name="Scala with Java 8" \
      io.openshift.tags="builder,scala,sbt" \
      io.openshift.min-memory="512Mi"

RUN curl -ns https://bintray.com/sbt/rpm/rpm > /etc/yum.repos.d/bintray-sbt-rpm.repo && \
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo && \
    yum update -y && \
    yum install -y java-1.8.0-openjdk-devel sbt docker-client && \
    echo "Installing openshift client ${OC_VER}..." && \
    set -o pipefail && curl -ns -L https://github.com/openshift/origin/releases/download/${OC_VER}/openshift-origin-client-tools-${OC_VER}-${OC_VER_COMMIT}-linux-64bit.tar.gz | \
    tar -zx && \
    mv openshift*/oc /usr/local/bin && \
    rm -rf openshift-origin-client-tools-* && \
    yum clean all -y && \
    rm -rf /var/cache/yum

# Copy test app to cache directory
COPY ./test/test-app /opt/test-app-root

# Cache SBT dependencies and plugins from test app
RUN cd /opt/test-app-root && \
    sbt -ivy /opt/app-root/src/.ivy2 -sbt-dir /opt/app-root/src/.sbt -sbt-boot /opt/app-root/src/.sbt/boot compile && \
    rm -rf /opt/test-app-root && \
    chown -R 1001:0 /opt/app-root && chmod -R ug+rwx /opt/app-root

COPY s2i/bin/ $STI_SCRIPTS_PATH

USER 1001

CMD $STI_SCRIPTS_PATH/usage
