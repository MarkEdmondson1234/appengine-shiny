# Appengine Shiny

In theory if you have a Docker container with a Shiny app within, it should be able to run on Google App Engine, where advantages include management of run time and launching on demand, rather than having to run a Shiny server all the time. 

## App engine how to

https://cloud.google.com/appengine/docs/flexible/custom-runtimes/build

## Deploy

https://cloud.google.com/appengine/docs/standard/python/tools/uploadinganapp

When in the same folder run

```
gcloud app deploy --project your-project
```

Only deploy to projects with app engine enabled for regions supported by flexible environment e.g. everywhere BUT europe-west :-/ - you may need to create a new Google project for this. 


## Debugging

Once deployed the app within is shown at https://your-project.appspot.com/shiny/ with the Shiny help page at https://your-project.appspot.com/

Current test server on https://mark-edmondson-usa.appspot.com/shiny/

Breaks due to Web Sockets not being proxied https://support.rstudio.com/hc/en-us/articles/213733868-Running-Shiny-Server-with-a-Proxy

https://github.com/rstudio/shiny-server/issues/155#issuecomment-231883143

The proxy used currently is this one:
https://github.com/GoogleCloudPlatform/appengine-sidecars-docker/tree/master/nginx_proxy

Nginx required:

```
http {

  map $http_upgrade $connection_upgrade {
      default upgrade;
      ''      close;
    }

  server {
    listen 80;
    

    
    location / {
      proxy_pass http://localhost:3838;
      proxy_redirect http://localhost:3838/ $scheme://$host/;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $connection_upgrade;
      proxy_read_timeout 20d;
    }
  }
}
``