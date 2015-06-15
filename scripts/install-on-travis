#!/bin/bash

# Add a 'bin' directory
mkdir -p $HOME/bin

# Create a local policy file that causes Travis to use 'drush 7' remotely.
# n.b. we want to use drush8 for Drupal 8 sites
mkdir -p "$HOME/.drush"
cat << __EOF__ > "$HOME/.drush/policy.drush.inc"
<?php

__EOF__

# Run composer install to install Drupal (pantheon-systems/drops-7),
# all of our modules, and other components we need
composer install
if [ "$?" != "0" ]
then
  echo "Composer install failed." >&2
  exit 1
fi

# Add vendor/bin and $HOME/bin to our $PATH
export PATH="$TRAVIS_BUILD_DIR/bin:$HOME/bin:$PATH"
