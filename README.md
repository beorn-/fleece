# Fleece
======

Fleece is a partial C rewrite of lumberjack... but lighter and non-blocking


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
            \"vhost\": \"www.test.gandi.net\" }" logstash_json

   CustomLog "|| /usr/bin/fleece --quiet --host logstash-vip.gandi.net --port 6784" logstash_json
   ErrorLog  "|| /usr/bin/fleece --quiet --host logstash-vip.gandi.net --port 6785 --field vhost=www.test.gandi.net --field role=frontend --field environment=test --field platform=website"
```

### CLI parameters

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
