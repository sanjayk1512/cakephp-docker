# PHP Docker

Create PHP Applications in a Docker Container

This setup spools up the following containers:

* **mariadb** (10.3)
* **nginx**   (latest)
* **php-fpm** (php 8.1 with almost all modules)
* **mailhog** (latest)
* **phpmyadmin** (latest)

To use different docker image tags:

* mariadb - use any version from `10.3` to `latest`
* nginx - use any version of `nginx:alpine`
* php-fpm - use any version from [phpdockerio/php72-fpm to phpdockerio/php81-fpm](https://hub.docker.com/r/phpdockerio/php)
* phpmyadmin - use any version of `phpmyadmin`

## Getting started

For those looking to get started in `60 sec` using just the defaults (which are fine for dev) do the following:

1. Create the following folder structure
 * Put your php app inside the `project` folder 
 * and the files from this repo into the `docker` folder

	```
		source/
		├── docker/ # git clone https://github.com/josephgodwinkimani/php-docker.git docker
		└── project/

	```

	If you want to do that all from commandline...

	```bash
    cd ~/Projects
    mkdir source
    mkdir source/project
	```

	And then to simultaneously download the latest master file, unpack it, stuff it into a folder named docker, and clone the `.env` file ... run this...

	```bash
    cd source
    curl -Lo docker.zip https://github.com/josephgodwinkimani/php-docker/archive/master.zip && \
    unzip docker.zip && \
    mv docker-master docker && \
    cp docker/.env.sample docker/.env
    rm docker.zip
	```
3. Change values in your `.env`

By default the `.env` file will contain the following

```
MYSQL_ROOT_PASSWORD=root
MYSQL_DATABASE=project
MYSQL_USER=project
MYSQL_PASSWORD=project
```

Docker Compose will automatically replace things like `${MYSQL_USER}` in the `docker-compose.yml` file with whatever corresponding variables it finds defined in `.env`

> You can configure **MySQL overrides** by editing `/mysql/my.cnf`

**Using mysql cli**

```bash
docker exec -it mysql /usr/bin/mysql -u root -p project
```

4. From commandline, `cd` into the `docker` directory and run `docker-compose up`

	```bash
	$ cd ~/Projects/source/docker
	$ docker-compose up
	```

	Here is an example of what my typical setup looks like

	```
		myapp-folder
			cakephp
				src
				config
				..
			docker
				.env
				.env.sample
				docker-compose.yml
				mysql
					my.cnf
				nginx
					nginx.conf
				php-fpm
					Dockerfile
					php-ini-overrides.ini
	```

5. Adding your first php project

If you're creating a new php app using a framework, follow the steps under **PHP Frameworks** below

Go to `localhost:8180` and your php app will be live.


6. Accessing your database

You can access your MySQL database from `phpmyadmin` on

`http://localhost:8080/`


7. Entering the PHP container

```bash
docker exec -it php-fpm /bin/bash
```

8. Accessing MailHog

This is just a built-in mail server you can use to 'send' and intercept mail coming from your application.

You can access the **Web GUI** (using the defaults) for mailhog at

`http://localhost:8125`


## PHP Frameworks

Most of the time I set this up without an existing CakePHP app. This will cause the initial `up` command to fail because folders are missing. To solve this run the install command (from the official CakePHP docs) but set the app name to `.` instead of `project`.

### Start a CakePHP/Laravel/Lumen/LeafPHP project

```bash
cd ~/Projects/source
docker exec -it php-fpm /bin/bash
```
and then, inside the PHP Container

```bash
composer create-project --prefer-dist cakephp/app:~4.0 . 
composer create-project --prefer-dist laravel/laravel . "5.6.*"
composer create-project --prefer-dist laravel/lumen .
composer create-project leafs/api .
```
Next, fix the database connection configurations in respective frameworks.

Visit `http://localhost:8180` or whatever you set nginx to respond to.
