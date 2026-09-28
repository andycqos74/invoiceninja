# nginx front-end for Invoice Ninja with the upstream config baked in,
# so the Portainer stack needs no bind-mounted files.
FROM nginx:alpine

RUN rm -f /etc/nginx/conf.d/default.conf
COPY invoiceninja.conf laravel.conf /etc/nginx/conf.d/
