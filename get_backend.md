Getting the code - backend
=====

Backend Installation
-----

Fetch the code (the forked code is compatibile with more recent software versions).

    git clone https://github.com/JilaRUIL/jila-backend.git

Install the gems. 

    cd jila-backend
    bundle install --without production

Update the database details in `config/database.yml` if you have a specific database setup. For development, the default values are OK.

Database
-----

Build the database.

    bundle exec rake db:migrate RAILS_ENV=development

If you need to reset it later, use `bundle exec rake db:reset db:migrate` 

Media
-----

Dev environment uses paperclip to store media locally (see the `config/environments/development.rb` file). This requires ImageMagick. See [the paperclip docs](https://github.com/thoughtbot/paperclip) for more info.

For production, media is stored on Amazon S3. To use this, we need an AWS account. Go to [AWS console](https://aws.amazon.com/console/) and create an AWS account, create a bucket, set permissions and CORS policy to allow the admin interface to put/post media.

- Bucket name: jila-LANGUAGE-media
- Permissions: Everyone can list, upload/delete
- Bucket CORS (copy this into the CORS section of the bucket properties):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <CORSRule>
        <AllowedOrigin>*</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
        <AllowedHeader>*</AllowedHeader>
    </CORSRule>
</CORSConfiguration>
```


Jila uses the AWS storage in production. So, back in the admin code, set the ENV vars for Paperclip in the `config/environments/production.rb` file. This is the code library that uploads media to AWS S3.

- AWS_BUCKET
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- AWS_REGION


Environment vars
-----

Add these ENV values using dotenv or set them in your local server config:
SECRET_KEY_BASE (used by config/secrets.yml)

*** Add more here about which files are edited, particularly for dotenv ***


Local branding
-----

Change the admin site title `config.site_title` in `config/initializers/active-admin.rb`.

Change the dashboard logo in `app/admin/dashboard.rb`.

Commit any outstanding changes.

    git commit -a -m "Localised the backend"



Run
-----

Run the backend locally.

    rails s 

- Open a browser to http://0.0.0.0:3000
- Default user is jila@gettingintouch.com.au
- Default password is birds123


Next
=====

See the [mobile](get_mobile.md) docs for steps on getting and customising the code.

See the [deploy doc](deploy.md) for info about publishing the app and backend.

