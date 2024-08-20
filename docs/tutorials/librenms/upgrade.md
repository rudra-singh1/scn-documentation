# Upgrading
Upgrading can be segmented into 2 parts. The container OSses and the service

## OS
Each container runs an OS
- The Maria db container is based on ubuntu, so you can just do `sudo apt update` and `sudo apt upgrade` when executing bash on the container
- The Redis container for some reason only has an ash executable installed on the container. Also it runs on alpine so it can be updated using `apk update` and `apk upgrade`
- The libreNMS and the libreNMS dispatcher container is instantiaated on the same container which is on alpine, and uses bash. Update as usual for alpine installs.

## Service
I think that libreNMS vends a script called daily.sh (librenms docs [here](https://docs.librenms.org/General/Updating/)) that is added to the cron, but cron is not running on docker containers since each container only runs one process. Even if we manually run daily.sh, there are errors. I think that the docs also give a manually manual way to update, by doing a git clone, but since the librenms files were not pulled using git, we cant use this way. I tried using rsync to overwrite the old files with the new files, but there are some issues. The nuclear option can be used, which is remove the containers, build new updated ones, and start that, but this will include a small outage

1. Go to the compose directory and run `sudo docker compose down` to stop and remove all the containers. We store data (rrd files and the database) in a docker volume inside the compose directory anyway so we should not need to worry about removing containers removing any data
1. Go to the `librenms_image` directory, change the version of the image to the latest version [here](https://hub.docker.com/r/librenms/librenms/tags)
1. run `sudo docker build . -t scn-librenms`, which should build the new image
1. go to the `db_image` directory and update the Dockerfile's version to the latest version [here](https://hub.docker.com/_/mariadb/tags)
1. run `sudo docker build . -t scn_mariadb_librenms`
1. go to the compose directory and update the compose.yml file to use the latest redis release [here](https://hub.docker.com/_/redis/tags)
1. Then you should be able to start the service with a `sudo docker compose -f compose/compose.yml up -d`
1. The service should come up as it was before. If it does not, you may have to do a [./lnms migrate](https://docs.librenms.org/General/Updating/)