# Appengine Shiny

In theory if you have a Docker container with a Shiny app within, it should be able to run on Google App Engine, where advantages include management of run time and launching on demand, rather than having to run a Shiny server all the time. 

## App engine how to

https://cloud.google.com/appengine/docs/flexible/custom-runtimes/build

## Deploy

When in the same folder run

```
gcloud app deploy --project your-project
```

Only deploy to projects with app engine enabled for regions supported by flexible environment e.g. everywhere BUT europe-west :-/ - you may need to create a new Google project for this. 