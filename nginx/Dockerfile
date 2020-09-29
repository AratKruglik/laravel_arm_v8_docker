FROM arm64v8/nginx:latest

ARG TZ=${WORKSPACE_TIMEZONE}

COPY ./nginx/certs/ /etc/nginx/certs
COPY ./nginx/sites-available /etc/nginx/sites-available/

WORKDIR /etc/nginx/

ENTRYPOINT ["/entrypoint.sh"]

CMD ["nginx", "-g", "daemon off;"]
