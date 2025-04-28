FROM nginx:1.27.5

COPY ./container/web/nginx.conf /etc/nginx/nginx.conf
COPY ./container/web/default.conf /etc/nginx/conf.d/default.conf
COPY --chown=nginx:nginx ./api /home/php/api

RUN touch /var/run/nginx.pid && \
  chown -R nginx:nginx /var/run/nginx.pid && \
  chown -R nginx:nginx /var/cache/nginx && \
  chmod 775 -R /var/run/nginx.pid /run/nginx.pid

EXPOSE 8080

USER nginx

WORKDIR /home/php/api