Getting the code - mobile
=====

Mobile Installation
-----

Fetch the code (the forked code is compatibile with more recent software versions).

    git clone https://github.com/JilaRUIL/jila-mobile.git

Change into the new directory.

    cd jila-mobile

Install the gems.

    bundle install

Doing bundle install for the first time on your machine might require the `--without production` flag, which is then stored in .bundle/config so does need to be typed for subsequent bundle installs/updates [see this stack overflow answer for more info](http://stackoverflow.com/a/19145217). If there are failures, try doing `bundle install --without production`, then run bundle install again.

Install bower, cordova and other tools using the provided rake task.

    bundle exec rake install_tools

Install Cordova plugins, ios & android platforms using the bootstrap rake task.

    bundle exec rake bootstrap


Testing
-----

Preview in browser using middleman to check that all has gone OK. We'll localise the app soon..

    bundle exec middleman server

Use the provided rake task to build the static site and run on emulator, or device if it is connected.

    rake ios:run

To use a specific emulator, view the list of available ones with

    cordova emulate ios  --list

And run this with specific model

    cordova emulate ios  --target="iPhone-4s, 9.3"


Localise the frontend
-----

## Config

Update the `source/config.xml` details with language name info.

    id: org.jila.birds.LANGUAGE
    version: INCREMENT THIS
    name:   NAME
    author: LANGUAGE

## Backend URL

Update the `js/configuration.coffee` backend url with the url of the heroku site. You can use the IP address of dev server to test locally. (Make sure the local backend server is running if you do this.)

    BACKEND_URL = 'http://HEROKU_APP_NAME.herokuapp.com' # production
    BACKEND_URL = 'http://0.0.0.0:3000'                  # local development

** Note: the local server is running on port 3000 whereas the frontend is on port 4567. For the frontend to display the images, we can use a URL forwarding rule. Install Chrome Switcheroo extension and add these redirect rules:

    http://bbb.local:4567/system/entries/ > http://0.0.0.0:3000/system/entries/
    http://0.0.0.0:4567/system/entries/ > http://0.0.0.0:3000/system/entries/

## Strings

Update the app title and other strings in `js/services/i18n_service.coffee`.

- splashTitle
- splashSubTitle
- homeTitle
- homeWordOfTheDay
- wordOfTheDayTitle

## Home page layout

Change home page layout - hide unwanted rows in `source/js/`partials/_home.haml

## Splash image

Replace the splash image at `source/img`

## About page content

Update content on  `source/js/partials/_about.haml`

Remember to include the app version `Version {{ appVersion }}` if about page content changes.

## Commit the changes

    git commit -a -m "Localise LANGUAGE"


Next
=====

Test it again

See the [deploy doc](deploy.md) for info about publishing the app and backend.

