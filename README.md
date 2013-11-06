fleece
======

Fleece is a partial C rewrite of lumberjack... but lighter and non-blocking

An apache2 vhost logformat configuration example
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
