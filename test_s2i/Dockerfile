# imgtest
FROM registry.access.redhat.com/ubi8/ubi:8.0

# TODO: Put the maintainer name in the image metadata
LABEL maintainer="Your Name user@ibm.com"

# TODO: Rename the builder environment variable to inform users about application you provide them
ENV BUILDER_VERSION 1.0 \
    PATH="/opt/rh/rh-nginx18/root/usr/sbin:$PATH" \
    NGINEX_DOCKROOT="/opt/rh/rh-nginx18/root/usr/share/nginx/html"

# TODO: Set labels used in OpenShift to describe the builder image
LABEL io.k8s.description="Platform for building nginx s2i builder image" \
      io.k8s.display-name="builder s2i nginx" \
      io.openshift.expose-services="8080:http" \
      io.openshift.s2i.scripts-url="image:///usr/libexec/s2i" \
      io.openshift.tags="builder,nginx,nginx18"

ADD nginxconf.sed /tmp/
COPY ./.s2i/bin/ /usr/libexec/s2i

# TODO: Install required packages here:
# RUN yum install -y ... && yum clean all -y
RUN yum install -y --nodocs rh-nginx18 \
    && yum clean all -y \
    && sed -i -f /tmp/nginxconf.sed /etc/opt/rh/rh-nginx18/nginx/nginx.conf \
    && chgrp -R 0 /var/opt/rh/rh-nginx18 /opt/rh/rh-nginx18 \
    && chmod -R g=u /var/opt/rh/rh-nginx18 /opt/rh/rh-nginx18 \
    && echo 'Hello from the Ngix S2I builder image' > ${NGINX_DOCROOT}/index.html

EXPOSE 8080

USER 1001


CMD ["/usr/libexec/s2i/usage"]

# TODO (optional): Copy the builder files into /opt/app-root
# COPY ./<builder_folder>/ /opt/app-root/

# TODO: Copy the S2I scripts to /usr/libexec/s2i, since openshift/base-centos7 image
# sets io.openshift.s2i.scripts-url label that way, or update that label
COPY ./s2i/bin/ /usr/libexec/s2i

# TODO: Drop the root user and make the content of /opt/app-root owned by user 1001
# RUN chown -R 1001:1001 /opt/app-root

# This default user is created in the openshift/base-centos7 image
USER 1001

# TODO: Set the default port for applications built using this image
# EXPOSE 8080

# TODO: Set the default CMD for the image
# CMD ["/usr/libexec/s2i/usage"]
