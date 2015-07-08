## About

This project is designed to be included from the **require** section of a Drupal site's `composer.json` file.  Doing this allows you to achieve the following things:

* Specify the Drupal modules, themes and libraries you use in your composer.json file, and build them with Composer.
* Automatically build components via Travis or Circle CI every commit.
* Use Behat to run tests on your site from Travis.
* Automatically deploy your site to your Pantheon dev environment, or some other branch, every time the tests pass.

All of this can be accomplished with only a few light files committed to your repository.

## Setup

Copy the contents of the `examples` directory to the root of your project, renaming files as appropriate:
```
  example.gitignore     -> .gitignore
  example.travis.yml    -> .travis.yml
  example.circle.yml    -> circle.yml
  example.behat.yml     -> behat.yml
  features              -> features
```
Of course, you will only need one of .travis.yml or circle.yml; keep the one you want to use, and remove the other.

You will also need a composer.json file for your project.  The project [example-drupal7-composer](https://github.com/pantheon-systems/example-drupal7-composer) can be used as a template to quickly create your own project; for Drupal 8, see [drupal-composer/drupal-project](https://github.com/drupal-composer/drupal-project).

## Configuration

You must customize the contents of these files to suit the needs of your project.  See the detailed instructions below; more information is also available in the comments inside each file.

#### Composer

Set the **name** and **description** in your composer.json file to something appropriate for your project.  

Customize the **require** section to contain the modules and themes needed for your project.  You might want to try using [drush composer-generate](https://www.drupal.org/project/composer_generate) to get started.  If you want to run your site on Pantheon, then you should keep "pantheon-systems/drops-7" as your main component; otherwise, you may replace this with "drupal/drupal" if you prefer.

The custom installers in the **require** section of your composer.json file control the way the components in your project are installed. Always keep these items at the top, so that they are available at the very beginning of the installation process.  Modules and themes listed before the custom installers might not install correctly.

#### Travis CI

Define the environment variables that identify your site in the **global** section of your .travis.yml **env**.  See the comments in the file for instructions, especially for the encrypted environment variables and encrypted private key file.  The encrypted items are only necessary if you want to push your site to Pantheon after every successful test run.

You will need to set up your repository to be tested by Travis in order to encrypt your Travis API key.

* Log in to https://travis-ci.org in your web browser 
* Click on the "+" next to "My Repositories"
* Find the repository you would like to configure to test. Navigate to the right organization, if necessary, and click "sync" if the repository was created recently.
* Enable the repository by clicking on the checkbox next to it.
# Push a commit to your repository to start a build.
* [Add an ssh key to your Pantheon account](https://pantheon.io/docs/articles/users/loading-ssh-keys/). You may use the same private key that you use with GitHub, if you wish.
* [Encrypt your private key for Travis](http://docs.travis-ci.com/user/encrypting-files/).
  * Name your private key `travis-ci-key`, and place it at the base of your project (this file is already in the starting .gitignore file).
  * Create your encrypted key with `cd travis && travis encrypt-file travis-ci-key`.
  * Copy the `openssl` line output by `travis encrypt-file` into your .travis.yml file, replacing the `openssl` line that already exists in the **after_success** section.
  * Commit the generated travis-ci-key.enc and all changes to your .travis.yml file.

The other parts of the example .travis.yml file should run without modification.  Note that the scripts run from the `bin` directory come from either the `scripts` directory of this project (which are copied to the `bin` directory when you require "pantheon-systems/travis-scripts" in your project's composer.json), or from some other component **require-dev** (e.g. behat).

See the [Travis CI documentation](http://docs.travis-ci.com/user/getting-started/) for instructions on how to set up GitHub integration, so that your code will be automatically tested on every commit.

Do not use Travis CI if you are using Circle CI.

#### Circle CI

Push-to-pantheon for Circle CI is not provided yet; the other parts of the example circle.yml file should run without modification.

See the [Circle CI documentation](https://circleci.com/docs/getting-started) for instructions on how to set up GitHub integration, so that your code will be automatically tested on every commit.

Do not use Circle CI if you are using Travis CI.

#### Behat

This sample is set up to run a single behat test that confirms that the name of the site was set correctly by `drush site-install`.  Note that the first part of the site name is set by the `SITE_NAME` environment variable that you customize in your .travis.yml file; the second part of the site name is set to `Travis Test Site` on Travis, and `Pantheon Test Site` on Pantheon.

See the [behat documentation](http://docs.behat.org/en/latest/) for further instructions on adding more tests to your project.

## Local Testing

Once setup is complete, doing local testing is a simple matter of:
```
$ composer install
$ ./bin/local-test
```
