# Deploying
I wrote a script to deploy libreNMS with the configuration that SCN uses. The repo is [here](https://github.com/abacef/scn-librenms-deploy-script/tree/main)

## Software requirements
Only tested on debian and ubuntu. Not sure what else it works on but it could work on other linux distros


## Steps
1. Install docker if it is not installed already
1. Install docker compose if it is not installed already
1. Instal unzip if it is not installed already
1. Check out this repo
1. If you want to restore a previous install, provide a sqldump named `librenms.sql` flat in this checked out repo. There is a helper script called `get_database_from_currently_running_server.sh` to get the database off of the non dockerized install (needs ssh access to the server)
   1. If you want to restore the graphs too, you can provide a file named `rrd.zip` flat in the checked out repo which is just the rrd folder ziped up.  There is a helper script called `get_rrd_zip_from_currently_running_server.sh` to get the rrd zip from the non dockerized install (needs ssh access to the server)
1. Run `./deploy.sh`
   1. builds the librenms image
   1. builds the database image with/without the backup
   1. Starts the service using `docker compose`. This creates 2 shared volumes in the `compose` directory
      1. The `librenms` folder is for the librenms docker images to share configuration data including rrd files
      1. The `db` volume is the database
   1. unzips the rrd folder in the rrd directory of the shared `librenms` volume

The UI will run on port 8000

