FROM debian:stable

MAINTAINER dv4all <d.mijatovic@dv4all.nl>

# install basic OS platform with required software
RUN apt-get update && apt-get upgrade -y && apt-get install -y apache2

# create envirnment variables that can be used during start of image
ENV TEST_VAL "my test value is stored here"

# expose post 80 to external world (so we can Sport forward on host)
EXPOSE 80

# run list of commands [start apache server]
CMD ["/usr/sbin/apache2ctl","-D","FOREGROUND"]
