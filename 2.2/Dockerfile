FROM centos/s2i-base-centos7

# This image provides a Ruby 2.2 environment you can use to run your Ruby
# applications.

EXPOSE 8080

ENV RUBY_VERSION 2.2

ENV SUMMARY="Platform for building and running Ruby $RUBY_VERSION applications" \
    DESCRIPTION="Ruby $RUBY_VERSION available as docker container is a base platform for \
building and running various Ruby $RUBY_VERSION applications and frameworks. \
Ruby is the interpreted scripting language for quick and easy object-oriented programming. \
It has many features to process text files and to do system management tasks (as in Perl). \
It is simple, straight-forward, and extensible."

LABEL summary="$SUMMARY" \
      description="$DESCRIPTION" \
      io.k8s.description="$DESCRIPTION" \
      io.k8s.display-name="Ruby 2.2" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,ruby,ruby22" \
      com.redhat.component="rh-ruby22-docker" \
      name="centos/ruby-22-centos7" \
      version="2.2" \
      release="1" \
      maintainer="SoftwareCollections.org <sclorg@redhat.com>"

RUN INSTALL_PKGS="rh-ruby22 rh-ruby22-ruby-devel rh-ruby22-rubygem-rake v8314 rh-ruby22-rubygem-bundler nodejs010 nodejs010-npm" && \
    yum install -y centos-release-scl && \
    yum remove -y rh-nodejs6\* && \
    yum install -y --setopt=tsflags=nodocs  $INSTALL_PKGS && rpm -V $INSTALL_PKGS && \
    yum clean all -y

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Copy extra files to the image.
COPY ./root/ /

RUN chown -R 1001:0 /opt/app-root && chmod -R ug+rwx /opt/app-root && \
    rpm-file-permissions

USER 1001

# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage
