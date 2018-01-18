# Symfony4 Windows Docker config

The current repository gives you a Docker config to run a Symfony4 application, with a bunch 
of services like : 
* **MySQL** database
* **PHP7** FPM to run Symfony
* **NGING** for the webserver
* **NodeJS** to use NPM for utilities like Gulp, Webpack or whatever you need.

## What do you need ?
You need to have **Docker** for Windows installed with **Docker-Compose.**  
And that's it.

## Removing MySQL
As Symfony4 is a skeleton without any Doctrine installed, you can remove the MySQL service
into the `docker-compose.yml` file.  
  
**Do not forget to remove the links to the MySQL container into the PHP service config !**


## Copy Docker configuration

If you already have a Symfony4 application, just clone the `docker/` folder and the `docker-compose.yml` file.
  
If you want to start a new Symfony4 project you can clone everything, and then read the last part 
of the README : "Instantiate Symfony4"

## Run it !
Just run the following commands on the folder root :
```sh
$ docker-compose build
```

It will create 4 containers : 
* symfony4-windock-mysql
* symfony4-windock-php
* symfony4-windock-nginx
* symfony4-windock-node

  
  Now run : 
```sh
$ docker-compose up -d
```
  
  
The created server runs at this local URI : 
```
http://symfony4-project.local
```
It's now your job to **modify your _hosts_ file** to avoid conflict with other projects
and containers, and let Windows do the match with loopback IP : 
```
# C:\Windows\System32\drivers\etc
 
127.0.0.1      symfony4-project.local
```

**Note** : we don't use the `.dev` domain because it doesn't work anymore with Google Chrome (redirecting 
all `.dev` URI to HTTPS).


## Important notes

Note that the Symfony app should be installed in the same root folder. The `docker/` 
folder will be at the same level as Symfony folders (`public/`, `var/`, `src/`, etc...).
  
Mysql will store all it's data into the `data/mysql/` folder, that's why I 
added it to the `.gitignore` file. Don't forget to do the same.


## Instantiate Symfony4
If you use it to create a new Symfony4 project, you can run composer into the PHP container.

First, remove the existing `public/` folders because Symfony will need to create it.

Then, run a bash into the php container : 
```
$ docker exec -t -i symfony4-windock-php /bin/bash
```
  
Into the command prompt opened, run the official Symfony4 composer creation command : 
```
root@...:/var/www/symfony# composer create-project symfony/skeleton tmpsf4
```
As you can see, we use a `tmpsf4/` folder because composer cannot create the project in 
our current directory because it's not empty.
  
Once the `create-project` instruction is over, let's move the Symfony files onto 
our current dir.
```
root@...:/var/www/symfony# mv tmpsf4/* .
```
  
The Symfony `var/` folder cannot be moved because Docker uses our `var/` folder.  
It's not a problem, let's remove it, Symfony will create new cache and log later.
```
root@...:/var/www/symfony# rm -rf tmpsf4/var/ && rm -f tmpsf4/.gitignore && mv tmpsf4/* . && mv tmpsf4/.env* . && rm -rf tmpsf4
```
  
Now if you browse `http://localhost/` you should be able to see the Symfony4 homepage !

