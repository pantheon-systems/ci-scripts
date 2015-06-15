#!/bin/bash
#
# setup-before-tests.sh
#
# The purpose of this script is to prepare a Drupal site to run
# the behat tests on.
#
# Usage:
#
#    compser install
#    ./travis/setup-before-tests.sh
#    ./bin/behat
#

SELF_DIRNAME="`dirname -- "$0"`"
PROJECT_BASE_DIR="`cd -P -- "$SELF_DIRNAME/.." && pwd -P`"

# Set up our $PATH
export PATH="$PROJECT_BASE_DIR/bin:$HOME/bin:$PATH"

# Fix bug in our custom installer
if [ -d $PROJECT_BASE_DIR/htdocs/sites/sites/default ]
then
  mv $PROJECT_BASE_DIR/htdocs/sites/sites/default $PROJECT_BASE_DIR/htdocs/sites/default
fi

if [ ! -f $PROJECT_BASE_DIR/htdocs/sites/default/default.settings.php ]
then
  echo "No default.settings.php file; did you run composer install?"
  exit 1
fi

# Do the settings.php shuffle for an empty settings.php
# This prevents permissions issues with the sites/default directory
cp $PROJECT_BASE_DIR/htdocs/sites/default/default.settings.php $PROJECT_BASE_DIR/htdocs/sites/default/settings.php

# Use Drush to install Drupal and spin up PHP's built-in webserver
cd $PROJECT_BASE_DIR/htdocs
drush site-install -y standard --site-name="$SITE_NAME Travis Test Site" --db-url=mysql://root@localhost/drupal --account-name=admin --account-pass=admin
if [ $? != 0 ]
then
  echo "Could not install Drupal" >&2
  exit 1
fi
drush runserver --server=builtin 8088 --strict=0 </dev/null &>$HOME/server.log &
if [ $? != 0 ]
then
  echo "Could nt start php built-in webserver" >&2
  exit 1
fi
cd $PROJECT_BASE_DIR

# Wait for the webserver to spin up; stop when we find someone listening on the right port.
echo "Waiting for the web server to finish spinning up."
until netstat -an 2>/dev/null | grep '8088.*LISTEN'; do sleep 0.2; done
echo "Webserver ready; continuing."
