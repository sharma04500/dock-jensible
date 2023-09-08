ARG DEBIAN_FRONTEND=noninteractive
ARG CACHEBUST=1
FROM ubuntu/apache2
COPY food1/* /var/www/html/
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
EXPOSE 80