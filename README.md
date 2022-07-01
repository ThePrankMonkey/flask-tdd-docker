# Flask-Docker

## Build

`docker-compose up -d`

`docker-compose up -d --build`

## Lint

```
docker-compose exec api black src
docker-compose exec api isort src
```

## Test

```
docker-compose exec api python -m pytest "src/tests"

docker-compose exec api python -m pytest "src/tests" -p no:warnings --cov="src"
```

## Heroku

Create a heroku app:

`heroku create`

```
Creating app... done, ⬢ tranquil-wave-08130
```

Note the output of the app's name. I'm going to set mine to an envionment variable.

`export HEROKU_APP=tranquil-wave-08130`

`heroku addons:create heroku-postgresql:hobby-dev --app $HEROKU_APP`

```
Creating heroku-postgresql:hobby-dev on ⬢ tranquil-wave-08130... free
Database has been created and is available
! This database is empty. If upgrading, you can transfer
! data from another database with pg:copy
Created postgresql-flat-97459 as DATABASE_URL
Use heroku addons:docs heroku-postgresql to view documentation
```

Build:

`docker build -f Dockerfile.prod -t registry.heroku.com/$HEROKU_APP/web .`

Test:

`docker run --name flask-tdd -e "PORT=8765" -p 5005:8765 registry.heroku.com/$HEROKU_APP/web:latest`

Push:

`docker push registry.heroku.com/$HEROKU_APP/web:latest`

Release:

`heroku container:release web --app $HEROKU_APP`

Create Prod DB:

```
heroku run python manage.py recreate_db --app $HEROKU_APP
heroku run python manage.py seed_db --app $HEROKU_APP
```

Test Prod:

curl https://$HEROKU_APP.herokuapp.com/ping

Check Logs:

```
heroku logs --app $HEROKU_APP
```

## Dummy

http --json POST https://$HEROKU_APP.herokuapp.com/users username=hello email=hello@world.com
curl -X POST https://$HEROKU_APP.herokuapp.com/users -H 'Content-Type: application/json' -d '{"username":"hello", "email":"hello@world.com"}'
