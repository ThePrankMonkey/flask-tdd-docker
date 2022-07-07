# Development Notes

Rebuild

```bash
docker-compose up -d --build
```

Linting, don't run as block as it doesn't seem to work...?

```bash
docker-compose exec api flake8 src
docker-compose exec api black src
docker-compose exec api isort src
```

Test

```bash
docker-compose exec api python -m pytest "src/tests" -p no:warnings
```

## Manual Testing

```bash
heroku run bash --app $HEROKU_APP
export FLASK_APP=src/__init__.py
flask shell
```

```bash
curl -s -X GET https://$HEROKU_APP.herokuapp.com/users | jq .
curl -s -X POST https://$HEROKU_APP.herokuapp.com/users -H 'Content-Type: application/json' -d '{"username":"someone", "email":"someone@something.com"}' | jq .
curl -s -X PUT https://$HEROKU_APP.herokuapp.com/users/4 -H 'Content-Type: application/json' -d '{"username":"foo", "email":"foo@bar.com"}' | jq .
curl -s -X DELETE https://$HEROKU_APP.herokuapp.com/users/4 | jq .
```

http --json POST https://$HEROKU_APP.herokuapp.com/users username=hello email=hello@world.com

## Odds and Ends

Generate SECRET_KEY:

```python
import random
import string

key_length = 50

# https://docs.gitlab.com/ee/ci/variables/index.html#mask-a-cicd-variable
chars = string.ascii_letters+string.digits+"@:.~"
secret = "".join([random.choice(chars) for _ in range(key_length)])

print(secret)
```
