## DockerTemplate (Docker template project to compose various projects)

## Installation

Copy .env from .env.example file and then write your database config (The example is made for mysql):

```bash
cp .env.example .env
```
Configure nginx host files as app-8081-example in ./docker/hosts :

```bash
1. Modify app-8081-example.conf or create your file. These are application-specific nginx parameters.
    - Set 'listen';
    - Set 'server_name';
    - Set 'root' of your index file;
    - Set 'error_log' path on your server;
    - Set 'access_log' path on your server;
    - Set '/shared/' -> 'root' path;
    - Set name of app in '/' -> 'try_files';
    - Set name of upstream in '@*name*' -> 'fastcgi_pass'; 
2. Modify app.conf. These are main nginx parameters.
    - Set 'server_name';
    - Set 'error_log' path;
    - Set 'access_log' path;
    - Set 'location' for specific application. This will allow you to do a redirect to the correct application at a specific routing. For example, to the back or to the front.
```

Configure docker images:

```bash
1. Database
    - Create your database folder in ./docker/images/database/ (as example mysql);
    - Create Dockerfile;
    - Configue docker image of your database in Dockerfile by https://hub.docker.com/ ;
2. Nginx
    - You can use the current image setup and nginx configuration or customize your own in the folder  ./docker/images/nginx/;
3. Application area
    - For example PHP in ./docker/images/php/. Php with mysql, xdebug and redis extentions. There's also a composer.
```

Configure projects configs & environments in ./docker/projects/ :

```bash
1. The 'example' project was the symfony project, so docker replaces the database connection with the one specified in .env.local.example

cd ./docker/projects/example    
cp .env.local.example .env.local
```

Configure docker-compose.yaml for your projects:

```bash
1. Configure app_database container for your database;
2. Configure containers of applications. app_exmaple as example;
3. Configure app_nginx container;
```
Run in terminal:

```bash 
docker-compose -f docker-compose.yaml up --build --force-recreate
```



All changes made in projects will be automatically pulled into the docker containers. If you change docker configs or images, you need to restart docker compose. 
To use XDEBUG (only for php projects), you need to configure it in the development area and in the browser. For example, in PHPSTORM and Google Chrome.




