# Docker image for the Monodevelop IDE
FROM phusion/baseimage:0.9.16

MAINTAINER Giles Paterson <giles@vurt.uk>

# Set correct environment variables.
ENV HOME /root

# Regenerate SSH host keys. baseimage-docker does not contain any, so you
# have to do that yourself. You may also comment out this instruction; the
# init system will auto-generate one during boot.
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh

# Install Monodevelop
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF && \
    echo "deb http://download.mono-project.com/repo/debian wheezy main" | tee /etc/apt/sources.list.d/mono-xamarin.list && \
    apt-get update && \
    apt-get -y install \
            monodevelop \
            monodevelop-database \
            monodevelop-nunit \
            monodevelop-versioncontrol \
            mono-xsp4

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir -p /home/giles && \
    echo "giles:x:1000:1000:Giles,,,:/home/giles:/bin/bash" >> /etc/passwd && \
    echo "giles:x:1000:" >> /etc/group && \
    echo "giles ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/giles && \
    chmod 0440 /etc/sudoers.d/giles && \
    chown giles:giles -R /home/giles

# Set workdir
# You'll need to run this image with a volume mapped to /app (i.e. -v $(pwd):/app) or override this value
WORKDIR /home/giles/projects

# Expose xsp4 server port
EXPOSE 8080

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]
