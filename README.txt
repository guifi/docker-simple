
Install docker server : https://docs.docker.com/install/#server
Install docker compose: https://docs.docker.com/compose/install/


## Clone the repository: DOCKER-SIMPLE together with the submodules: drupal-guifi, drupal-budgets, theme_guifinet2011 and guifimaps.
git clone --recurse-submodules https://github.com/guifi/docker-simple.git
cd docker-simple

## Build the composition.
sudo docker-compose build
3 dockers have been built:
 - apache2-simple 10.6.0.2
 - mysql-simple 10.6.0.3
 - phpmyadmin-simple 10.6.0.4
A common network has been created 10.6.0.0/24

# change www folder permissions to uid:gid 33:33 (www-data)
sudo chown -R 33.33 www

# start the simple dockers.
sudo docker-compose up

go to:
 - http://YOUR_DOCKER_SERVER:18001 for the drupal WEB environment
  The first time you must configure the database for drupal
  Set this values: ( You can modify it before the build process in the file docker-compose.yml )
  Database name: drupal
  Database username: drupal
  Database password: GUIFI
  unfold the Advanced option
  Replace "Database host" from "localhost" to "mysql-simple or 10.6.0.3"

  When drupal is configured vist : http://YOUR_DOCKER_SERVER:18001/admin/settings/error-reporting
  Replace "Error reporting:" to Write errors to the log.
 
  Then you can enable the "guifi.net" module in http://YOUR_DOCKER_SERVER:18001/admin/build/modules.
  Create your first ZONE without "Parent zone" at http://YOUR_DOCKER_SERVER:18001/node/add/guifi-zone
  Get the node ID  (XXX) from the new created master zone ( http://YOUR_DOCKER_SERVER:18001/node/XXX ).
  If the ID is different from 1, edit the file confs/guifimaps/refresh.php and set the variable: $rootZone the value of XXX

  Go to http://YOUR_DOCKER_SERVER:18001/admin/settings/guifi
   set your own "Key for Google Maps API"
   and "URL for WMS service" to http://YOUR_DOCKER_SERVER:18001/cgi-bin/mapserv?map=/var/www/guifimaps/GMap.map

  Now you can create child zones and first nodes and devices. A CRON job will update the WMS layer every 5 minutes.


 - http://YOUR_DOCKER_SERVER:18002 for the phpmyadmin environment

 - The mysql process is listening only internally on port 3306.
