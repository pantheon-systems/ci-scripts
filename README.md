## About

This project is designed to be included from the **require-dev** section of a Drupal site's `composer.json` file.  Doing this allows you to achieve the following things:

* Specify the Drupal modules, themes and libraries you use in your composer.json file, and build them with Composer.
* Automatically build components from within a Travis build on every commit.
* Use Behat to run tests on your site from Travis.
* Automatically deploy your site to your Pantheon dev environment, or some other branch, every time the tests pass.

All of this can be accomplished with only a few light files committed to your repository.

## Setup

Copy the contents of the `examples` directory to the root of your project, renaming files as appropriate:
```
  example.composer.json -> composer.json
  example.gitignore     -> .gitignore
  example.travis.yml    -> .travis.yml
  example.behat.yml     -> behat.yml
  features              -> features
```
The project [example-drupal7-composer](https://github.com/pantheon-systems/example-drupal7-composer) can be used as a template to create your own project.

## Configuration

You must customize the contents of these files to suit the needs of your project.  See the detailed instructions below; more information is also available in the comments inside each file.

#### Composer

Set the **name** and **description** in your composer.json file to something appropriate for your project.  

Customize the **require** section to contain the modules and themes needed for your project.  You might want to try using [drush composer-generate](https://www.drupal.org/project/composer_generate) to get started.  If you want to run your site on Pantheon, then you should keep "pantheon-systems/drops-7" as your main component; otherwise, you may replace this with "drupal/drupal" if you prefer.

The **require-dev** section contains the components needed by this project (including a reference to this project itself as the first item).  If you alter any of the selections here, you will fundamentally alter the way these scripts operate; only do this if you really understand all of the implications.

#### Travis

Define the environment variables that identify your site in the **global** section of your .travis.yml **env**.  See the comments in the file for instructions, especially for the encrypted environment variables and encrypted private key file.  The encrypted items are only necessary if you want to push your site to Pantheon after every successful test run.

The other parts of the example .travis.yml file should run without modification.  Note that the scripts run from the `bin` directory come from either the `scripts` directory of this project (which are copied to the `bin` directory when you require "pantheon-systems/travis-scripts" in your project's composer.json), or from some other component **require-dev** (e.g. behat).

See the [Travis documentation](http://docs.travis-ci.com/user/getting-started/) to set up GitHub integration, so that your code will be automatically tested on every commit.

#### Behat

This sample is set up to run a single behat test that confirms that the name of the site was set correctly by `drush site-install`.  Note that the first part of the site name is set by the `SITE_NAME` environment variable that you customize in your .travis.yml file; the second part of the site name is set to `Travis Test Site` on Travis, and `Pantheon Test Site` on Pantheon.

See the [behat documentation](http://docs.behat.org/en/latest/) for further instructions on adding more tests to your project.

## Local Testing

Once setup is complete, doing local testing is a simple matter of:
```
$ composer install
$ ./bin/local-test
```
