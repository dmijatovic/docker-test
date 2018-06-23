# create your custom drupal image here, based of official drupal

FROM drupal:8.2

# update image, install git clean install files
RUN apt-get update && apt-get install -y git \
    && rm -rf /var/lib/apt/lists/*

#change working directory to themes
WORKDIR /var/www/html/themes

# clone drupal bootstrap project theme
# change permission from root to www-data 
RUN git clone --branch 8.x-3.x --single-branch --depth 1 https://git.drupal.org/project/bootstrap.git \
    && chown -R www-data:www-data bootstrap

# back to www root dir
WORKDIR /var/www/html




