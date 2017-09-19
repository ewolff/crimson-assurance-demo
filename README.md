# Docker Container for Crimson Insurance Demo

This project provides Docker containers for the Crimson demo, see
https://github.com/crimson-assurance . This project uses a fork of the
original code

It consists of four services: portal, letter, notify claims and
postbox.

To start run `docker-compose up -d`. This will also build
the Docker images.

https://crimson-portal.herokuapp.com/ is a running instance of the
application that is deployed on Heroku.

The Docker containers should run on `localhost`. Otherwise set the
environment variable `CRIMSON_SERVER` to the host the containers run on.

Points to note:

* There are some customers - try Mei or Mül etc
* The postbox is transcluded from the server running on port 3001
* The link to write a letter or notify a claim will go to different
  applications (on port 3002 and 3003)
* Behind the scenes a project with the shared assets ensure common UI
elements.
* The Postbox is written in Java / Spring Boot, the other projects in NodeJS.

This project is based on a very similar project by @mjansing and
@moonglum .

The source code for the services can be found at:

* https://github.com/ewolff/crimson-portal
* https://github.com/ewolff/crimson-damage
* https://github.com/ewolff/crimson-letter
* https://github.com/ewolff/crimson-postbox

The asset project is https://github.com/ewolff/crimson-styleguide .
