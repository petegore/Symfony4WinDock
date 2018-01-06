# Symfony4 Windows Docker config

The current repository gives you a Docker config to run a Symfony4 application, 
but I also added some dev tools like Coke, or Make, in a separate container.

## What do you need ?
To run this, you need to have Docker for Windows and Docker-Compose installed.
And that's it.

## Run it !
Just run the following commands on the folder root :
```bash
$ docker-compose build
```

It will create 3 containers : PHP, MySQL and NGINX.  
  
  Now run : 
```bash
$ docker-compose up -d
```
  
  
The created server runs at this local URI : 
```
http://symfony4-project.dev
```
It's now your job to **modify your _hosts_ file** to avoid conflict with other projects
and containers, and let Windows do the match with loopback IP : 
```shell
# C:\Windows\System32\drivers\etc
 
127.0.0.1      symfony4-project.dev
```


## Notes

Note that the Symfony app should be installed in the same root folder. The `docker/` 
folder will be at the same level as Symfony folders (`public/`, `var/`, `src/`, etc...).
  
Mysql will store all it's data into the `data/mysql/` folder, that's why I 
added it to the `.gitignore` file. Don't forget to do the same.


## Instantiate Symfony4
If you use it to create a new Symfony4 project, you can run composer into the PHP container : 