FROM registry.fedoraproject.org/fedora:33
#RUN yum -y install java-1.8.0-openjdk
ENV MONGO_DB="sling"
ENV REPOSITORY="sling/repository"
ENV KARAF_DEBUG=true
RUN mkdir -p /opt/pantheon-karaf-dist

RUN dnf install -y --best\
    --setopt=install_weak_deps=False \
    java-1.8.0-openjdk-devel \
    glibc-langpack-en \
    which \
    procps-ng \
    golang

ENV LANG=en_US.UTF-8

RUN mkdir /build
COPY . /build
WORKDIR /build

RUN chmod +x scripts/deploy_local.sh
RUN PANTHEON_CODEBASE=/build scripts/deploy_local.sh

RUN tar --strip-components=1 -C /opt/pantheon-karaf-dist -xzf /build/pantheon-karaf-dist/target/pantheon-karaf-dist-*.tar.gz

RUN chmod 755 /opt; 
#sed -i "21s/out/stdout/" /opt/pantheon-karaf-dist/etc/org.ops4j.pax.logging.cfg;

# expose JMX HTTP DEBUG ports
EXPOSE 1099 8181 5005

CMD  ["/opt/pantheon-karaf-dist/bin/karaf", "run"]
