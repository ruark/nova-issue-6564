FROM nginx:1.24-alpine

RUN rm -f /etc/nginx/conf.d/*
COPY conf/ /etc/nginx/conf.d/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
