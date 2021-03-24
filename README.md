# codex-reverse-proxy-service
This service acts as a reverse proxy to the sparql endpoint https://codex.opendata.api.vlaanderen.be:8888/ .

The service was developed for use in a [mu-project](https://github.com/mu-semtech/mu-project) but you should be able to use it standalone as well.

It provides an endpoint `/sparql` for querying. Note that it will only pass the following headers to the underlying endpoint:

* Accept
* Accept-Encoding
* Accept-Language
* Method
* Content-Type
* Content-Length
* Pragma
* Cache-Control

As such it will strip both semantic.works specific headers as well as any header that may be added by `ember serve`.

## including this service in a mu-project
You can use the following block in your docker-compose file:

```
services:
  ...
  codex-proxy:
    image: lblod/codex-reverse-proxy-service
```

Example configuration of [mu-dispatcher](https://github.com/mu-semtech/mu-dispatcher)

```
  match "/codex/sparql/*path" do
    forward conn, path, "http://codex-proxy/sparql/"
  end
```
