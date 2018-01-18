# docker/node/Dockerfile
#See https://github.com/nodejs/docker-node#dockerfile
FROM node:6

EXPOSE 8080

USER node

# set the working directory
RUN mkdir /home/node/app
WORKDIR /home/node/app

# delete existing modules and re-install dependencies
COPY package.json /home/node/app/package.json
COPY package.lock /home/node/app/package.lock
RUN rm -rf node_modules
RUN npm install
