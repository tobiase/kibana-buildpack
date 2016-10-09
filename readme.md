# Scalingo buildpack for Kibana

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

That's it your Kibana is live, you may have to wait a few seconds to let kibana
create its indexes, then refresh and the dashboard will be available.

## Plugins

You may want to install plugins to your Kibana installation like
[Sense](https://www.elastic.co/guide/en/sense/current/installing.html).  To do
that, just create a file `kibana-plugins` with the names of the plugins you
wish to install. If you need to install a plugin from a custom URL, just
specify it after the name of the plugin.

Example of `kibana-plugins` file:

```
elastic/sense
logtrail https://github.com/sivasamyk/logtrail/releases/download/0.1.4/logtrail-4.x-0.1.4.tar.gz
```

## Security

If bother environment variables `KIBANA_USER` and `KIBANA_PASSWORD` are
defined, we'll deploy *nginx* alongside *Kibana*. All the requests will be
authenticating by nginx before being proxied to Kibana. The latter is not
longer directly reachable from the Internet.

## Extra configuration

* `DOWNLOAD_URL`: Source of the kibana archive, default is: `https://download.elastic.co/kibana/kibana/kibana-4.6.1-linux-x86_64.tar.gz`
