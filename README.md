# Symfony4 Windows Docker config

The current repository gives you a Docker config to run a Symfony4 application, with a bunch 
of services like : 
* **MySQL** database
* **PHP7** FPM to run Symfony
* **NGINX** for the webserver

I decided to remove the Node container I added in version 1.3 because it's not
necessary for Symfony app.
  
### What do you need ?
You need to have **Docker** for Windows installed with **Docker-Compose.**  
And that's it.
  
### Step 1/4 - Remove MySQL if you don't want it
As Symfony4 is a skeleton without any Doctrine installed, you can remove the MySQL service
into the `docker-compose.yml` file.  
  
**Do not forget to remove the links to the MySQL container into the PHP service config !**
  
  
### Step 2/4 - Run docker-compose
Just run the following commands on the folder root :
```sh
$ docker-compose build
$ docker-compose up -d
```

### Step 3/4 - Update your hosts file
  
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
  
  
**Note 2** : for the moment the URI should show a "File not found" message.


### Step 4/4 - Instantiate Symfony4
If you use it to create a new Symfony4 project, you can run the official Symfony4 creation command into the 
php container.  
  
**NOTE** : I didn't put it into the php Dockerfile as CMD, because it would have instantiate a Symfony project 
each time you started your containers.

```sh
$ docker exec -it -u www-data -w /var/www/symfony symfony4-windock-php /bin/bash -c "composer create-project symfony/skeleton app"
```

Now if you browse `http://symfony4-project.local/` you should be able to see the Symfony4 homepage !

### Important notes

Note that the Symfony app should be installed in the `app/` folder in order to distinguish Docker folders 
and application folders. Some folders, as `var/` can appear in both of them.  
The folder structure is the following : 

```
current_folder
+-- app (created by composer create-project)
    +-- config
    +-- public
    +-- src
    +-- var
+-- data
    +-- mysql
    +-- (other containers data if needed)
+-- docker
    +-- nginx
    +-- php7-fpm
+-- var
    +-- log
    +-- (other docker containers var data)
````
  
Mysql will store all it's data into the `data/mysql/` folder, that's why I 
added it to the `.gitignore` file. Don't forget to do the same.



