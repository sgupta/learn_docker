#Location of source image
FROM alpine:3.7
RUN apk add --no-cache nginx && apk add curl && mkdir -p /var/www
COPY ./.config/nginx.conf /etc/nginx/nginx.conf
COPY index.html /var/www/index.html
EXPOSE 80
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
