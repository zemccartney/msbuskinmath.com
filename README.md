# msbuskinmath.com

- Teaching portfolio website
- Simple wordpress install
- hosted / operationalized with Google Cloud Platform
- https://console.cloud.google.com 
- https://ms-buskin-math-wordpress.appspot.com/ 

## Installation

### Requirements

- Access to project's GCP account :)

### Process

1. Make a top-level directory to house everything in the project e.g. `mkdir mbm-wp` and `cd` into that
2. Follow Google's instructions
    - In the 4th step, set the path the WP project's created in to within `mbm-wp`
    - https://cloud.google.com/php/tutorials/wordpress-app-engine-flexible
    - https://torbjornzetterlund.com/setting-wordpress-google-app-engine/ (almost identical to Google's, but also documents some known gotchas that tripped up first setup)
    - Doublecheck that DB instance names in config and env (`app.yaml` and `wp-config.php`) files have region listed; caused errors on first setup, not sure why region wasn’t written
    
You should now have a directory structure like this:

```sh
mbm-wp/
|-- service.json
|-- php-docs-sample/{{ contents of google dir }}
|-- wp_dir # Name you gave to the project in step 4 of Google's instructions
|     |-- app.yaml
|     |-- composer.lock
|     |-- composer.json
|     |-- cron.yaml
|     |-- nginx.-app.conf
|     |-- php.ini
|     |-- vendor/{{ php modules }}
|     |-- wordpress/{{ standard Wordpress directory structure }}
```

3. `cd wp_dir/wordpress`
4. Overwrite default `wp-content` with contents of this repo
    1. `git init`
    2. `git remote add origin git@github.com:zemccartney/msbuskinmath.com.git`
    3. `git pull origin master`
5. Deploy the site: `cd mbm-wp` & `gcloud app deploy` (deployment must happen in wordpress project folder i.e. where `app.yaml` if present)
    - This can take a bit of time (few minutes or so)

6. Activate the Google Storage plugin in the WP admin
    - This plugin comes with install; must be activated to allow uploading Media; switches media uploads from file system (which is not writable in App Engine; see https://cloud.google.com/appengine/docs/standard/php/googlestorage/ ) to Google Cloud Storage:
https://github.com/GoogleCloudPlatform/wordpress-plugins/tree/master/appengine-plugin

7. Make Google Cloud Storage bucket accessible to site, both from links and scripts
    - Bucket can be made public-readable through UI: https://cloud.google.com/storage/docs/access-control/making-data-public
    - the site's [PDF Embedder Premium plugin](https://wp-pdf.com/) requests PDFs from Google Cloud Storage programmatically, which means we need to configure CORS on the bucket. We do this
    by creating a CORS configuration JSON file, then setting it on the bucket
        - `gsutil cors set cors-config.json gs://{{ bucket name }}`
        
## Notes on Maintaining

- All plugins and themes must be installed by adding files to local file system, then redeploying
    - E.g. download zip files, place in corresponding subdirectories in wp-content, then gcloud app deploy from project root

- All SQL access is through the gcloud sql proxy, including updating plugins

    1. Start SQL Proxy in 1 terminal tab
        - `cloud_sql_proxy -dir /tmp/cloudsql -instances={{ instance name, nabbed from GCP portal }}=tcp:3306 -credential_file={{ path to service account JSON file }}`
    2. Open another terminal tab
    3. Run plugin updating commands `vendor/bin/wp plugin update --all --path=wordpress`
        - Run from WP project root (`./wp_dir` from the above treeview)
    
   
## Resources

- Git repo for wp install from instructions : https://github.com/GoogleCloudPlatform/php-docs-samples/tree/master/appengine/wordpress
- What is App Engine? https://stackoverflow.com/questions/22697049/what-is-the-difference-between-google-app-engine-and-google-compute-engine 
