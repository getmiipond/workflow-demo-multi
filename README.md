## Setup

```
# install postgres into namespace `services`
helm install --name postgres --namespace services kubernetes-charts/postgresql --set postgresUser=demo-app,postgresPassword=secret,postgresDatabase=demo-app

# install redis into namespace `services`
helm install --name redis --namespace services kubernetes-charts/redis --set redisPassword=redisSecret

# create application in workflow
deis create demo-app

# configure application to use postgres
deis config:set DATABASE_URL="postgres://demo-app:secret@postgres-postgresql.services.svc.cluster.local/demo-app"

# configure application to use redis
deis config:set REDIS_URL="redis://:redisSecret@redis-redis.services.svc.cluster.local"

# configure application to use custom buildpack
deis config:set BUILDPACK_URL="https://github.com/slack/heroku-buildpack-ruby.git#env-from-files"

# push the application
git push deis master

# setup the database
deis run bundle exec rake db:migrate
```


