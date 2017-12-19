# Scalingo buildpack for Kibana

This buildpack downloads and installs Kibana into a Scalingo app image.

## Compatibility

Tested against Kibana 5.5.0 - ES 5.5.0

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

## Elasticsearch Configuration

### HTTPS with self-signed certificate

Use the environment variable `ELASICSEARCH_TLS_CA_URL` to specify an URL to
download the certificate from like
[https://db-api.scalingo.com/api/ca_certificate](https://db-api.scalingo.com/api/ca_certificate).

Alternatively you can add the CA to your GIT repository and configure its path
with the variable `ELASTICSEARCH_TLS_CA_PATH` (example: `ca.crt`)

## Plugins

You may want to install plugins to your Kibana installation like
[logtrail](https://github.com/sivasamyk/logtrail). To do
that, just create a file `kibana-plugins` with the urls of the plugins you
wish to install.

Example of `kibana-plugins` file:

```
https://github.com/sivasamyk/logtrail/releases/download/v0.1.18/logtrail-5.5.0-0.1.18.zip
```

## Plugins configuration

You may want to configure your plugins. To do that, just create a file 'plugins-config' with the local path of your config file and the path where this config file should be stored in the plugins directory.

Example of 'plugins-config' for the 'logtrail.json' file:

```
logtrail.json:logtrail/logtrail.json
```

## Security

If bother environment variables `KIBANA_USER` and `KIBANA_PASSWORD` are
defined, we'll deploy *nginx* alongside *Kibana*. All the requests will be
authenticating by nginx before being proxied to Kibana. The latter is not
longer directly reachable from the Internet.

## Extra configuration

* `DOWNLOAD_URL`: Source of the kibana archive, default is: `https://artifacts.elastic.co/downloads/kibana/kibana-5.5.0-linux-x86_64.tar.gz`
