FROM quay.io/dennybaa/droneunit:trusty
# this image is based on https://github.com/tianon/dockerfiles/blob/master/sbin-init/ubuntu/upstart/Dockerfile.template
#

COPY init-fake.conf /etc/init/fake-container-events.conf

# undo some leet hax of the base image
RUN echo -e "#!/bin/sh\nexit 101\n" > /usr/sbin/policy-rc.d && \
  rm /sbin/initctl; dpkg-divert --rename --remove /sbin/initctl

# generate a nice UTF-8 locale for our use
RUN locale-gen en_US.UTF-8 && update-locale LANG=en_US.UTF-8

# remove some pointless services
RUN /usr/sbin/update-rc.d -f ondemand remove; \
  for f in \
    /etc/init/u*.conf \
    /etc/init/mounted-dev.conf \
    /etc/init/mounted-proc.conf \
    /etc/init/mounted-run.conf \
    /etc/init/mounted-tmp.conf \
    /etc/init/mounted-var.conf \
    /etc/init/hostname.conf \
    /etc/init/networking.conf \
    /etc/init/tty*.conf \
    /etc/init/plymouth*.conf \
    /etc/init/hwclock*.conf \
    /etc/init/module*.conf\
  ; do \
    dpkg-divert --local --rename --add "$f"; \
  done; \
  echo '# /lib/init/fstab: cleared out for bare-bones Docker' > /lib/init/fstab


# let Upstart know it's in a container
ENV container docker

# we can have SSH
EXPOSE 22

# pepare for takeoff
CMD ["/sbin/init"]
