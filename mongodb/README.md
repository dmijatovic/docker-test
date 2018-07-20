# MongoDB Docker compose container

This project creates a mongodb instance and mounts data into the data folder.

## NPM scripts

```bash
  # start mongodb and mongo-express containers on mongo_net network in detached mode
  npm start

  # stop container
  npm run stop

```

## Docker setup

The setup is defined in docker-compose.yml file. We start mongodb container and expose default port 27017 to localhost. This port can be used by any app to create connection to mongodb. The data is saved to data folder ()


## How to use it

Basic use is setting up mongodb and node.js web-based admin portal by executing `npm start`. This will enable mongodb on default port 27017 and admin interface on port 8081 of local machine (localhost).

