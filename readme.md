# scalingo buildpack for Kibana

This buildpack downloads and installs Kibana into a Scalingo app image.

## Compatibility

Tested against Kibana 4.5.4 - ES 2.3.4

## Usage

To deploy your own Kibana, you've to do the following:

```console
$ git init
$ scalingo create my-kibana
$ scalingo env-set BUILDPACK_URL=https://github.com/Scalingo/kibana-buildpack

# If you don't already have an elasticsearch instance from another app
$ scalingo addons-add scalingo-elasticsearch free
# If you already have the ES instance, refer its URL
$ scalingo env-set ELASTICSEARCH_URL="http://user:password@host:port"

$ echo 'web: kibana --port $PORT' > Procfile
$ git add Procfile
$ git commit -m "Prepare Kibana for Scalingo deployment"
$ git push scalingo master
```

That's it your Kibana is live, you may have to wait a few seconds to let kibana create its indexes, then refresh and the dashboard will be available.

## Extra configuration

* `DOWNLOAD_URL`: Source of the kibana archive, default is: `https://download.elastic.co/kibana/kibana/kibana-4.6.1-linux-x86_64.tar.gz`
