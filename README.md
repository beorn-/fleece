# Fleece
======

Fleece is a partial C rewrite of lumberjack... but lighter and non-blocking. This tool was written for our logstash platform at Gandi.net. The tool reads stdin then if it is already a JSON stream it just allows you to forward it through UDP to a remote host, if it's a plain text stream fleece jsonifies it and pushes it to the remote host.

## Configuration

### Apache2

An apache2 vhost logformat configuration example:

```ApacheConf
   LogFormat "{ \
            \"@timestamp\": \"%{%Y-%m-%dT%H:%M:%S%z}t\", \
            \"@version\": \"1\", \
            \"clientip\": \"%a\", \
            \"duration\": %D, \
            \"status\": %>s, \
            \"message\": \"%U%q\", \
            \"urlpath\": \"%U\", \
            \"urlquery\": \"%q\", \
            \"bytes\": %B, \
            \"method\": \"%m\", \
            \"referer\": \"%{Referer}i\", \
            \"useragent\": \"%{User-agent}i\", \
            \"platform\": \"website\", \
            \"role\": \"frontend\", \
            \"environment\": \"test\", \
            \"vhost\": \"vhost.domain.tld\" }" logstash_json

   CustomLog "|| /usr/bin/fleece --quiet --host host.domain.tld --port 6784" logstash_json
   ErrorLog  "|| /usr/bin/fleece --quiet --host host.domain.tld --port 6785 --field vhost=www.test.gandi.net --field role=frontend --field environment=test --field platform=website"
```

### Logstash

A logstash minimal configuration to get those events indexed

```
input 
  [...]

  #fleece
  udp {
    port => 6784
    type => "json"
    tags => ["apache", "access_log", "fleece"]
    codec => "json"
  }

  udp {
    port => 6785
    type => "json"
    tags => ["apache", "error_log", "fleece"]
    codec => "json"
  }
  [...]
}

```

### CLI parameters

And the --help output of fleece

```Bash
Usage: ./fleece [options]]
  --help                  show this help
  --version               show the version of fleece
  --field VALUE           Add a custom key-value mapping to every line emitted
  --host VALUE            The hostname to send udp messages to
  --port VALUE            The port to connect on the udp server
  --quiet                 Disable outputs
  --window_size VALUE     The window size
```

## Build debian package

We are using debian for our platforms. This repository embeds the debian packaging information.
