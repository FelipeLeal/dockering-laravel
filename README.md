# Dockerizing Laravel 
> Sets up a **LEMP** network of containers for local Laravel development.

## Getting Started
These instructions will cover usage information and for the docker container 

### Prerequisites
In order to run this container you'll need **Docker** installed.
* [Docker on Windows](https://docs.docker.com/windows/started)
* [Docker on OS X](https://docs.docker.com/mac/started/)
* [Docker on Linux](https://docs.docker.com/linux/started/)


### Usage
- First add your entire Laravel project to the `src` folder, then open a terminal and from this cloned repository's root 
run:
```bash
docker-compose up -d --build nginx
```

- **Your Laravel app needs to be in the src directory first before 
bringing the containers up, otherwise the artisan container will not build, as it's missing the appropriate file.** 


Containers created, and their ports (if used) are as follows:

- **nginx** - `:8080`
- **mysql** - `:3306`
- **php_fpm** - `:9000`

Three additional containers are included that handle Composer, NPM, and Artisan commands without having to have these platforms installed on your local computer. Use the following command examples from your project root, modifying them to fit your particular use case.:
- **npm**
- **composer**
- **artisan**

Update composer dependencies
```bash
docker-compose run --rm composer update
```
Run all Mix tasks - [Running Mix](https://laravel.com/docs/7.x/mix#running-mix) -
```bash
docker-compose run --rm npm run dev
```
To run all of your outstanding migrations - [Running Migrations](https://laravel.com/docs/7.x/migrations#running-migrations) -
```bash
docker-compose run --rm artisan migrate
```

Open up your browser of choice to:
> [http://localhost:8080](http://localhost:8080)

- The port need to be set in `docker-compose` and `.env`.
Now you should see your Laravel app running as intended. 

### Laravel - Mysql
To set up the **database connection** from the environment file `.env` first you need to set the host to your mysql container 
name like `DB_HOST=mysql-service-name` 

### MySQL Dumps
Exec `mysql_config_editor` in the mysql container to store the root password in `--login-path`, after executed, the 
command line will request the password
```bash
docker-compose exec mysql mysql_config_editor set --login-path=local --host=localhost --user=root --password
```

Then exec `mysqldump` from the mysql container replacing `MYSQL_ROOT_PASSWORD` and `MYSQL_DATABASE` your `docker-compose.yml` data
```bash
docker-compose exec mysql /usr/bin/mysqldump --login-path=local -e {MYSQL_DATABASE} > dump.sql
```

## Multiple Docker Laravel
If you need to have multiple Laravel projects running at the same time, you must be replaced the containers names and 
choose another port for these:
```
## Project 1
nginx:
    image: nginx:stable-alpine
    container_name: nginx_1
    ports:
      - "8080:80"

## Project 2
nginx:
    image: nginx:stable-alpine
    container_name: nginx_2
    ports:
      - "8081:80"
```

## License
[MIT](https://choosealicense.com/licenses/mit/)