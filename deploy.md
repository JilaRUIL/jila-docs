Deploying the backend and publishing the mobile apps
=====

To run the admin site on Heroku, we'll set up a Heroku app which will use the code from the latest Git commit. [Read more at the heroku docs.](https://devcenter.heroku.com/articles/git#tracking-your-app-in-git)


Backend
=====

The backend must be under Git version control, it should be already from the installation steps. Make sure all your changes have been committed.

Set up Heroku access
-----

Create an account on Heroku if you don't have one already.

Install Heroku with `brew install heroku-toolbelt`.

Test login from the command line.

    heroku login


Make a Heroku app
-----

Create a new Heroku app. [More reading here](http://sourabhbajaj.com/mac-setup/Heroku/README.html)

This adds the remote address of the heroku repository to the git repo.

    heroku apps:create jila-LANGUAGE-admin    

To name the remote something other than 'heroku', do

    heroku apps:create jila-LANGUAGE-admin --remote REMOTENAME

Push the current branch to Heroku remote. This command is in the format: git push remote branch. Might need forced update `-f` if local gets ahead of origin/master.

    git push heroku master

Push from a particular branch with:

    git push REMOTENAME BRANCHNAME:master

Create the database on Heroku.

    heroku run rake db:migrate --remote REMOTENAME

Add the app's ENV vars to Heroku. Get the AWS details from earlier.

    heroku config:set AWS_BUCKET=jila-LANGUAGE-media --app jila-LANGUAGE-admin
    heroku config:set AWS_ACCESS_KEY_ID=############ --app jila-LANGUAGE-admin
    heroku config:set AWS_SECRET_ACCESS_KEY=########### --app jila-LANGUAGE-admin

Launch.

    heroku open --app jila-LANGUAGE-admin




Frontend
=====

Publish Android app
-----

### For Android

Build icons using the rake icons task. This runs a conversion script. Put the 1024x1024 icon in the app-icon folders then run the task. Replace FILENAME with the filename of the 1024x1024 icon. 

    rake android:build_icons[FILENAME]


Prepare android key (see Keystore/Automatic signing method section in the platform-android.md doc). Might need to change the signing properties for this app to match the one it was published with (`release-signing.properties` file in the `platforms/android` dir).

Get back into the project dir, then use the rake task to build it for release.

    rake android:release

(If this fails with a permission error, you may need to do `bundle exec rake android:release`.)

First time testing on this Android device? Set it up for testing by:

- Go to Settings > About Phone. Press the build number info seven times. 
- Now go into Settings > Developer Options and enable USB debugging. This allows the app to be installed onto the device over USB cable. 
- Plug in USB cable

Sanity check before uploading to the store. Install the apk on the device using the adb command. Check the device, you may need to approve the computer's connection.

    adb install -r platforms/android/build/outputs/apk/android-release.apk


To publish the app on the Google Play Store, sign in and upload the apk, icons and images. 

*** More detail needed about Google Play steps ***


### For Apple

Build the Xcode project. You'll need to open this once in Xcode to set the signing property.

    rake ios:release

If the build fails, remove the platform and add it again as a first step in finding the problem.

## Update with our app's icons and splash screens

Create a 1024x1024 icon. Use something like [Asset Catalog Generator](https://langui.net/asset-catalog-generator/) to generate an Asset Catalogue. Do the App Icons first, save them and then do the Launch Image, save into the folder that ACG created. Launch image is best as something very plain, don't use the splash bgd.

Now replace the default Xcode asset images with these new ones by dragging the Images.xcassets directory that ACG buildt into `platforms/ios/APPNAME/` dir.

## Get screenshots for the Apple Store listing

Open the Xcode project, run it in various simulators to get screenshots. Take iOS device screenshots using Xcode Devices window. Zoom has to be 100% to get the right sizes.

- iPhone 3+4  3.5 Inch    640 x 960
- iPhone 5    4 Inch      640 x 1136
- iPhone 6    4.7 Inch    750 x 1334
- iPhone 6+   5.5 Inch    1242 x 2208
- iPad        Air & Mini  1536 x 2048

## Enrol in Apple developer program

[apple.com](apple.com)

** More info needed here **

# Create new app at itunesconnect.apple.com

Set up the new app in itunesconnect.apple.com, add details, upload screenshots.

New App
- Name:   Thipa Pertame Birds
- Primary Language: Australian English (!)
- Bundle ID: XC Wildcard *
- Bundle ID suffix: org.jila.birds.pertame
- SKU: app01 

App Information
- Category: Reference

Pricing
- Free

App version
- What's new in this app
- Screenshots
- Description: Arrernte birds. A Getting In Touch app
- Keywords: Arrernte, birds, Getting In Touch, JILA
- Support URL: gettingintouch.com.au
- Copyright info: All rights reserved
- Rating: 
- App icon: 1024x1024 icon
- App review information
- Demo Account: Not required

## Set up certificates in Xcode

Open `Xcode > Preferences > Accounts`

Add Apple dev account to the accounts list by signing in

Click `View details`

Click `Create` for each of the iOS Development and Distribution certificates

Download all

## Set up provisioning profile

Connect device. Build onto that to generate a provisioning profile.

In Xcode, view account details again to check the provisioning profile exists, 

## Team 

On the General tab, check that the project has a team assigned.

## Create archive 

With device connected, create an app archive for uploading `Product > Archive` 

If you get a profile error, [check this](http://stackoverflow.com/questions/32821189/xcode-7-error-missing-ios-distribution-signing-identity-for )

Open the Organiser window, select the app in the LH side.

Select the latest build in the middle panel.

Check it first - click `Validate` in the RH panel.

Click `Upload the App Store`

In the popup, choose the team.

If it verifies OK, click the `Upload` button.


## Updating?

When updating, add a new verion in itunesconnect first, then upload with Xcode





Notes
=====

If Android icons don't show after rebuilding, may need to remove the android platform, add it again and copy icons in again. (Get back into the project directory, then...)

    cordova platform remove android
    cordova platform add android

To replicate the App store update process, get the ipa from /platforms/ios/build/device. Rename it to include the version number. Then update the code, run the app again using rake ios:run. You should now have two ipa files - old one and update. Install or run the original. Then drag the update onto iTunes. With device plugged in there should be an option to update the app.


TLDR: here without comments
=====

    git checkout -b kune
    # make any config changes
    git commit -a -m "Localise for Kune"
    heroku apps:create jila-kune-admin --remote kune
    git push kune kune:master
    heroku run rake db:migrate --remote kune
    heroku config:set AWS_BUCKET=jila-kune-media --app jila-kune-admin
    heroku config:set AWS_ACCESS_KEY_ID=AKIAIONOA6K33Y66R4TQ --app jila-kune-admin
    heroku config:set AWS_SECRET_ACCESS_KEY=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 --app jila-kune-admin
    heroku open --app jila-kune-admin

    git checkout -b burarra
    # update content and config as needed
    # id: org.jila.birds.burarra
    # backend url: http://jila-burarra-admin.herokuapp.com
    git commit -a -m "Localise Burarra"
