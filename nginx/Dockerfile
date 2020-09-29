FROM arm64v8/nginx:latest

ARG TZ=Europe/Kiev

COPY ./certs/ /etc/nginx/certs
COPY ./sites-available /etc/nginx/sites-available/

WORKDIR /etc/nginx/

ENTRYPOINT ["/entrypoint.sh"]

CMD ["nginx", "-g", "daemon off;"]
