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
http://symfony4-project.local
```
It's now your job to **modify your _hosts_ file** to avoid conflict with other projects
and containers, and let Windows do the match with loopback IP : 
```shell
# C:\Windows\System32\drivers\etc
 
127.0.0.1      symfony4-project.local
```


## Notes

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
..
Now if you browse `http://localhost/` you should be able to see the Symfony4 homepage !

