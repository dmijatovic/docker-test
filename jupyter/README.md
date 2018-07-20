# Jupyter notebook, Docker and machine learning

This is demo of basic Jupyter notebook for data science as docker image where content is bound to local work folder using bind volume.

You need to have docker installed to use this setup.

## Docker Jupyter image

This project uses docker jupyter/base-notebook image. Then it runs linux update and install number of data science packages used in the projects.

NOTE! You might also use other jupyter images avaliable on docker hub to achieve the same goal. I wanted to experiment with docker to create specific image I need, however this image is quite close (by size and content) to jupyter/tensorflow-notebook image. I did not installed few pip packages note there.

## NPM runs

I am quite used to store scripts in package.json and use npm run instead of typing the commands you can run all required commands using NPM. There are two commands defined

- `npm run build-image:` will create docker image
- `npm start`: will start jupyter noteook on you localhost on port 8888. After image is started


## Project directory

Your project files should be stored in work directory. Each project can have its own subdirectory. These will be listed in jupyter notebook file explorer.
