FROM camilocot/base_druid:v2

EXPOSE 8081
# Setup supervisord
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ENTRYPOINT export HOSTIP="$(getent hosts $HOSTNAME | awk '{ print $1 }')" && exec /usr/bin/supervisord -c /etc/supervisor/conf.d/supervisord.conf
