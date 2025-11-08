FROM nginx:1.27-alpine

COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80
HEALTHCHECK CMD wget -qO- http://localhost/health || exit 1
