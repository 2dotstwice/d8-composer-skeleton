# Introduction

This is a skeleton project meant to help you set up a new Drupal 8 project quickly.

# Creating a fork

Because this skeleton is meant to help you get started once, any changes you make later should have no effect on the skeleton itself. 
Because of this you should create a fork of this repository, which will then serve as the repository of your new project.

To do so, clone this repository and remove the included `.git` directory to delete the commit history. Afterwards, run `git init` to set up the project as a new git repository.

It is also recommended remove the `composer.lock` entry from the included `.gitignore` file so the `composer.lock` file that will be generated later can be added to your project's new git repository. 
We ignore it by default so it would never get committed to the skeleton itself by accident, but it is considered best practice to always commit your `composer.lock` file in the actual Drupal 8 project itself.

# Installing dependencies

Next, install Drupal core and any other dependencies using Composer:
 
    composer install
    
If this is the first time you run `composer install` in the cloned repository, a new `composer.lock` file will be generated. Now would be a good time to commit it to your own repository.

Note that both Drupal core, and any other dependencies that were installed, are not tracked by git. Instead, you should always run `composer install` whenever you make a new clone of your project's repository.

In case you run `composer install` when you already have a `composer.lock` file, you have to scaffold some Drupal files manually by running the following command after running `composer install`:

    composer drupal-scaffold
    
Or, you can turn it into a one-liner like this:

    composer install && composer drupal-scaffold
    
# Configuring the vhost

The docroot of your vhost should point to the included `web` directory. This is where Drupal core will be installed, and `index.php` will be located.

# Contrib modules and themes

To install extra contrib modules or themes (hosted on [drupal.org](http://www.drupal.org)), simply add them to the list of Composer dependencies:

        "require": {
            ...
            "drupal/devel": "1.x-dev"
        }
        
Contrib modules and themes are installed in the following locations respectively:

* `web/modules/contrib`
* `web/themes/contrib`
    
Contrib modules and themes are not tracked by git. You should always update or re-install them using `composer update` or `composer install` (depending on the situation).

# Custom modules and themes

You can store custom modules and themes respectively in the following locations:

* `web/modules/custom`
* `web/themes/custom`

While generally everything inside `web` is ignored by git, these two locations are exceptions and will be tracked by git so you can commit any custom modules and themes to your git repository.

# Private modules and themes

You can install custom modules and themes stored in a private git repository using Composer.

To do so, add the module or theme as a dependency in `composer.json`, and add the private repository so Composer knows where to download the code from:

    "require": {
        ...
        "vendor/my-custom-module-or-theme": "~0.1"
    },
    "repositories": [
        {
            "type": "vcs",
            "url": "git@github.com:vendor/my-custom-module-or-theme.git"
        }
    ]
    
To make sure the specified module or theme gets downloaded to the correct directory, make sure it has either `drupal-custom-module` or `drupal-custom-theme` as its package type in its own `composer.json` file.
For example:

    {
        "name": "vendor/my-custom-module",
        "description": "A custom module hosted on a private repository",
        "type": "drupal-custom-module",
        "license": "...",
        "authors": [ ... ],
        "extra": {
            "branch-alias": {
                "dev-master": "0.x-dev"
            }
        }
    }
    
Private modules and themes are installed in the following locations respectively:

* `web/modules/private`
* `web/themes/private`

Alternatively, you can install your private modules and themes in the same location as contrib modules and themes by setting the package type of your module or theme to `drupal-module` or `drupal-theme` respectively.
 
Private modules and themes are not tracked by git. You should always update or re-install them using `composer update` or `composer install` (depending on the situation).
