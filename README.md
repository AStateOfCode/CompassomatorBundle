The Compassomator
=================
A bundle to simplify the development with Compass/SASS and symfony2 bundles. It is intended to be used in conjunction with Assetic and not as a replacement. In addition, it adds a file importer to SASS that is able to resolve the familiar **@BundleName** notation and also the SASS script function **bundle-public('@BundleName/images/foo.png')** to build the correct path to any asset in the bundles public directory.

# The Idea
Use compass to manage every bundle as a standalone project and use a special file importer to reference assets in other bundles. The generated CSS files are then referenced in Assetic.

# Requirements
Any Symfony2 2.3+ application will do.

# Installation
```shell
composer require asoc/compassomator-bundle
```

# Configuration
The Compassomator is also able to trigger *assetic:dump* and *assetic:dump --watch* if it is desired. To enable it, set the following option:

```yaml
asoc_compassomator:
	manage_assetic: true
```

# Quick Setup

## Step 1
Create a simple config.rb file inside every bundle inside the **Resources/** directory with the following content:

```ruby
css_dir = "public/css"
sass_dir = "sass"
```

Any generated CSS file will be placed inside the bundles **Resources/public/css** directory, every SCSS/SASS file that should be compiled by Compass will be searched in *Resources/sass*.

## Step 2
Add the **Resources/public/css** to **.gitignore** in the project root. In this setup it is assumed that there are *NO* css files in the public/css directory that are not generated by Compass/SASS. If there are, it might be required to choose another *css_dir*.

## Step 3
Reference the generated CSS files with the usual Assetic helper in the view:

```twig
{% stylesheets
'@BundleName/Resources/public/css/foo.css'
'@BundleName/Resources/public/css/bar.css'
%}
...
{% endstylesheets %}
```

## Step 4
Run the compassomator.

```shell
app/console compassomator:compile
```

If assetic is not run automatically (manage_assetic: true), dumping the assets with assetic is also required:

```shell
app/console assetic:dump
```

## Step 5
Run compass watch and assetic dump in the background to automatically update the generated CSS.

```shell
# Start
app/console compassomator:watch
# End
app/console compassomator:watch --abort
```

If assetic is not run automatically (manage_assetic: true), watching the assets with assetic is also required:

```shell
app/console assetic:dump --watch
```

## Step 6
Show logs

To view any errors, compass or assetic, the logs can be shown using the logs command.

```shell
app/console compassomator:logs
```

Logs and other run files can be found in **app/cache/compassomator** for debugging purposes.

# Thanks
Made possible by [BITE GmbH](https://www.b-ite.de) as a side project during my master thesis.

# Notes
- At the moment, cache:clear will trigger the compassomator:compile command once, so it will take a few seconds longer on a cache:clear by default.

# Contribution
Whatever is on your mind, open an issue or a pull request (be it a bug/typo/feature request/code improvement...) :)

# License
MIT
