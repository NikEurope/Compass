

Compass Doc

Compass is an open-source CSS authoring framework which uses the Sass stylesheet language to make writing stylesheets powerful and easy. If you're not familiar with Sass, Go to sass-lang.com to learn all about how it works.

Compass Help : http://compass-style.org/help/


-------------------------------------------

Utiliser Compass: 

Dossier sur VM avec le fichier   config.rb   
ex: /var/www/public/drupal/themes/myboot >>>  config.rb
> compass -v ( vérifier la version )
> compass compile  ( compiler le SASS avec CSS )
> drush cr  ( nettoyer le cache )


-------------------------------------------

Ex de config.rb   :

http_path = "/"
css_dir = "css" 
sass_dir = "assets/sass"
images_dir = "assets/images" 
javascripts_dir = "js" 

fonts_dir = "bootstrap/fonts/bootstrap" 
generated_images_dir = "img" 


--------------------------------------------



Blog - Sevaa Group

    Return to Sevaa.com

Build a Drupal 8 Bootstrap Subtheme with SASS
Combine a starter kit and Compass to make a Drupal 8 Bootstrap subtheme more powerful.
April 28, 2016 by Dan Hansen

You are excited about Drupal 8! You love Bootstrap! You appreciate the power SASS offers! So naturally you want to use the Drupal 8 Bootstrap theme and base a subtheme on it using SASS. There are unfortunately only two starter kit options that come packed in to the base theme: one for CDN and one for LESS. But we can leverage those subthemes and build exactly the configuration you want! This is a ground-up guide on how to implement that.

If you’re not already aware, the Drupal 8 Bootstrap theme has a lovely documentation page which this article borrows from for it’s installation portions. It’s a powerful reference that can be used to bridge the gap between Drupal and Bootstrap docs. You can find it at http://drupal-bootstrap.org/api/bootstrap/8

Additionally, this post is pretty closely based on an article written by Roland Michael dela Peña that did something very similar in the Drupal 7 version. You can read that original piece here: Using SASS in Bootstrap Drupal theme
Baseline Installation

This guide makes more than a few assumptions, but the key assumptions are:

    You have installed a working copy of Drupal 8
    Ruby is also installed on your machine, so you can get and install Compass if you don’t already have it to compile your SASS.

It’s also recommended that you have:

    Drush, because drush is great and if you do development for Drupal you need drush to run awesome CLI stuff.

With all that in place, we can get started with the first step.
Installing Drupal 8 Bootstrap with SASS
Step 1: Aquire Drupal 8 Bootstrap theme

You can get Drupal 8 Bootstrap pretty simply by running drush dl bootstrap in your Drupal root directory (known from here on as drupalroot ) if drush is installed. If not, you can pull the theme from the Drupal.org project page, then unzip the directory into drupalroot/themes/bootstrap

Note: At the time of this writing, Drupal 8 Bootstrap is still at 3.0-beta3. I don’t expect much change as long as the Bootstrap core on the theme remains at 3, but Bootstrap 4 is in alpha and I don’t know the roadmap, so that may change soon.
Step 2: Make your custom Drupal 8 Bootstrap subtheme

Most of the instructions in this step can be found in the READMEs scattered throughout the Drupal 8 Bootstrap theme as well as the official documentation page, but I’ve tried to be a little more straightforward here so it runs a little faster.

Drupal 8 Bootstrap comes loaded with two “starterkit” themes for you to create a child or subtheme with. You can find them in drupalroot/themes/bootstrap/starterkits . For this, you’ll want to copy the less  theme and paste it directly into the themes folder at drupalroot/themes . Rename the folder to your theme name, so the path should look something like drupalroot/themes/mytheme , where mytheme  is the machine name of your theme.

Now comes the fun part: renaming everything in the theme to match mytheme . Here’s the hit list of things to rename and otherwise alter inside of the folder drupalroot/themes/mytheme , changing THEMENAME to your theme’s name:

    config/install/THEMENAME.settings.yml to config/install/mytheme.settings.yml  Key to note in this file is the cdn_provider: '' setting that disables the Drupal 8 Bootstrap theme’s automatic use of a CDN.
    config/schema/THEMENAME.schema.yml to config/schema/mytheme.schema.yml Make sure you update the three spots where THEMENAME and THEMETITLE appear in this file as well.
    THEMENAME.libraries.yml  to mytheme.libraries.yml
    THEMENAME.starterkit.yml  to mytheme.info.yml  Note that starterkit changes to info. Edit this file to set your theme’s name and description. Finally be sure to update the two THEMENAME instances in the libraries section.
    THEMENAME.theme  to mytheme.theme

File structure comparison before and after Step 2

With those files altered, you should have a working Drupal 8 Bootstrap subtheme. Navigate on your Drupal site to /admin/appearance, scroll down the page and click ‘Install and set as default’ on your new theme.

Appearance configuration page showing how to enable the new theme

TADA! Your theme is ready and running!

New theme enabled...with no styling

Well, OK, kinda. There’s no styling, but that’s because we don’t have anything in place to compile the LESS code, and that is fine because we’re about to breath some life into the system with SASS!
Step 3: Bolt on Bootstrap SASS

Let’s put some SASS in this theme!
Step 3A: Get your theme organized

Start by removing the drupalroot/themes/mytheme/less  folder from the theme. You won’t be needing it any longer. In its place, you’ll want to add a few new directories to drupalroot/themes/mytheme:

    assets  Will hold all your new editables
    assets/images  Will hold the files to be turned into sprites using Compass
    assets/sass  Will hold all of your custom SCSS files
    bootstrap  Will carry all of the Bootstrap SASS source
    img  Will hold your compiled images/sprites
    js  Carries your compiled JavaScript

File structure comparison before and after Step 3A
Step 3B: Drop your SASS in place

Download a copy of Bootstrap SASS. There’s also a link directly to the download on the Bootstrap Getting Started page, under the heading Download > SASS.

When you decompress the Bootstrap SASS archive, the only parts you want are in the assets folder. Pick up everything in that folder and paste it into your new theme’s bootstrap folder at drupalroot/themes/mytheme/bootstrap.

Moving Boostrap SASS Assets
Step 3C: Customize Your SASS

In the bootstrap folder you just created, you’ll want to retrieve two files and copy them to your assets/sass folder:

    drupalroot/themes/mytheme/bootstrap/stylesheets/_bootstrap.scss to drupalroot/themes/mytheme/assets/sass/_bootstrap.scss
    drupalroot/themes/mytheme/bootstrap/stylesheets/bootstrap/_variables.scss to drupalroot/themes/mytheme/assets/sass/_variables.scss

Once those are in place, rename assets/sass/_bootstrap.scss to assets/sass/style.scss. This is now your core SASS file, and all of the includes need to be updated. Use find and replace in your editor to change “bootstrap/” to “../../bootstrap/stylesheets/bootstrap/” for all instances.

Finally, you want to find the line in style.scss that imports variables and change it to use the copy in the assets folder. It should be one of the first import lines in the file. Change it so that @import "../../bootstrap/stylesheets/bootstrap/variables"; becomes @import "variables";.

Text changes for style.scss in step 3C

With this all of your SASS files are in place and ready to be compiled.
Step 3D: Configure Compass to work that SASS

Since you (hopefully) already have Ruby installed, you should be able to navigate to your mytheme  directory and install Compass for use:

shell gem update --system gem install compass

This will automatically install SASS for you as well, if it’s not already there.

Normally you’d have compass do the dirty work of getting a project started for you, but since the configuration is a little peculiar we’ll build our own configuration. Create a file in mytheme  called config.rb . Add this information to it:

ruby http_path = "/themes/mytheme" css_dir = "css" sass_dir = "assets/sass" images_dir = "assets/images" javascripts_dir = "js" fonts_dir = "bootstrap/fonts/bootstrap" generated_images_dir = "img" http_images_path = http_path + "/" + generated_images_dir http_generated_images_path = http_images_path output_style = (environment == :production) ? :compressed : :expanded

Take care that the http_path matches your theme name.
Your SASS-powered Theme is Live!

With that in place you should be able to run compass compile  and compass watch  in your theme and get compiled CSS! Hooray progress! Any changes in your assets/sass directory should be included in the compiled CSS. This also means you can add SASS partials in the folder, include them in the style.scss file you created, and create a lavish ecosystem of overridden styles for your new theme.

To see your newly compiled styles in action, be sure to clear your caches by running drush cache-rebuild, or if you don’t have drush installed visit /admin/config/development/performance on your site and clear caches there.

Drupal Performance configuration page

You may also want to go ahead and disable CSS and JS caching while on that page during development.

Now refresh your homepage and see your styles in action!

Home page with Boostrap installed

It’s a little bare and it’s clear that some of the menu styles are still not 100%, but you’ll be able to handle those minor issues with no problems now that you have SASS in place on your Drupal 8 Bootstrap theme.

    ← Previous Post
    Next Post →

Sevaa Group  •  2016

Theme by beautiful-jekyll

