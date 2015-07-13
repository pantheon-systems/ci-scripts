## About

This project contains scripts useful for testing a Composer Drupal project with Behat on a Continuous Integration server, or locally.

* Specify the Drupal modules, themes and libraries you use in your composer.json file, and build them with Composer.
* Automatically build components via Travis or Circle CI every commit.
* Use Behat to run tests on your site from Travis.
* Automatically deploy your site to your Pantheon dev environment, or some other branch, every time the tests pass.

All of this can be accomplished with only a few light files committed to your repository.

See [travis-scripts](https://github.com/pantheon-systems/travis-scripts) and  [circle-scripts](https://github.com/pantheon-systems/circle-scripts) for specific instructions on how to use this project.
