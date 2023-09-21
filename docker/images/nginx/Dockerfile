FROM nginx

RUN apt -y update
RUN apt -y --no-install-recommends install curl unzip nginx
RUN apt -y --no-install-recommends install telnet net-tools iputils-ping mc

COPY ./nginx.conf /etc/nginx/nginx.conf

WORKDIR /etc/nginx
CMD ["nginx"]

EXPOSE 8080
