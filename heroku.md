`npm install -g heroku`

`heroku --version`
`heroku login -i`
`heroku config -a petclinic-be` : get db url for the heroku app.
heroku pg -a petclinic-be  : get more info of postgres db.
heroku addons : see db addon. 
heroku logs -t -a petclinic-be  : see logs of app running.
heroku logs -p postgres -t -a petclinic-be  : 
heroku pg:diagnose -a petclinic-be
heroku pg:info :info about postgres in heroku
heroku pg:psql -a petclinic-be : connect local postgres to heroku postgres.
