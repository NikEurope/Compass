

#Compass Doc

Compass is an open-source CSS authoring framework which uses the Sass stylesheet language to make writing stylesheets powerful and easy. If you're not familiar with Sass, Go to sass-lang.com to learn all about how it works.

Compass Help : http://compass-style.org/help/


-------------------------------------------

##Utiliser Compass: 

Dossier sur VM avec le fichier   config.rb   
ex: /var/www/public/drupal/themes/myboot >>>  config.rb
> compass -v ( vérifier la version )
> compass compile  ( compiler le SASS avec CSS )
> drush cr  ( nettoyer le cache )


-------------------------------------------

##Ex de config.rb   :

http_path = "/"
css_dir = "css" 
sass_dir = "assets/sass"
images_dir = "assets/images" 
javascripts_dir = "js" 

fonts_dir = "bootstrap/fonts/bootstrap" 
generated_images_dir = "img" 


--------------------------------------------




Build a Drupal 8 Bootstrap Subtheme with SASS

https://sevaa.com/blog/build-drupal-8-bootstrap-subtheme-sass/



--------------------------------------------

##Intégration graphique avec Sass

Installation :

Ruby:

sudo yum install ruby ruby-devel -y

Compass:

gem install compass

gem install bootstrap-sass

Compilation: dans le dossier du thème lancer la compilation

cd themes/cnas

compass compile


Pour forcer la suppression des fichiers css et la recompilation des fichiers scss :

cd themes/cnas compass clean && compass compile




