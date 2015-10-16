# opencollective-api
OpenCollective's API

## How to get started

### Database
Install Postgres 9.x. Start the database server, if necessary.

For development, ensure that local connections do not require a password. Locate your `pg_hba.conf` file by
running `ps aux | grep postgres` and note the directory in the `postgres` or `postmaster` process, specified with `-D`.
It will look something like `/Library/PostgreSQL/9.3/data`. We'll call this the `$POSTGRES_DATADIR`. `cd` to `$POSTGRES_DATADIR`, and
edit `pg_hba.conf` to `trust` local socket connections and local IP connections. Restart `postgres` - on Mac OS X, there may be
restart scripts already in place with `brew`, if not use `pg_ctl -D $POSTGRES_DATADIR restart`.

Now, assuming the postgres superuser is `postgres`, let's create the databases.
```
createdb opencollective_localhost
createdb opencollective_test
createuser philmod
psql -U postgres
> GRANT ALL PRIVILEGES ON DATABASE opencollective_localhost TO philmod
> GRANT ALL PRIVILEGES ON DATABASE opencollective_test TO philmod
```

### Configuration and secrets
- From the OpenCollective DropBox: `cp $DROPBOX/Engineering/config/DOTenv .env`
- There are other config files there, but for now they seem to be duplicated in `config`

### Node and npm

`npm install`

If you haven't already: `export PATH=./node_modules/bin:$PATH`. You probably want to add
that to your shell profile.


## Tests
`npm test`
All the calls to 3th party services are stubbed using either `sinon` or `nock`.

## Start server
`npm run start`

## Documentation
http://docs.opencollective.apiary.io/

## Databases migrations
The tests delete all the database's tables and re-create them with the latest models.

For localhost or other environments, the migrations has to be run manually.

### Create a new migration file
`sequelize migration:create`

### Apply migrations locally
`sequelize db:migrate --config config/default.json --models-path app/models`

### Apply migrations on other environments
Here for preview:
`sequelize db:migrate --config config/default.json --models-path app/models --env preview`

### Undo a migration
`sequelize db:migrate:undo --config config/default.json --models-path app/models`
