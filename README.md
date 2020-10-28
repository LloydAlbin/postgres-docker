# postgres
Custom builder of the Official PostgreSQL for Alpine Linux for use with TimescaleDB

The Official PostgreSQL Alpine version is creaded by default without LDAP support. We need the LDAP support, so this repository contains the code to build the PostgreSQL Alpine with LDAP support and a few additional extensions.

```bash
cd ~
# Get the postgres-docker repository
git clone https://github.com/LloydAlbin/postgres-docker.git
```

## Build Custom PostgreSQL Docker Image

The make command takes some optional options:

* org
* ts_name
* pg_name
* location
* push
* clean
* postgres

These options help define the docker image names in this format:

* org/pg_name:VERSION aka lloydalbin/postgres:11-alpine
* location aka ~ meaning ~/postgres and ~/timescaledb-docker for the two repositories needed to be downloaded
* --push aka push docker image(s) to the repsoitory
* --clean aka delete the two repositories
* --postgres aka build only postgres

If you have your own inhouse docker registery, then the ORG name should be the name of your inhouse docker registry.

The build script will download the postgres and timescaledb-docker repositories.

```bash
# For the first time
~/postgres-docker/build_timescaledb.sh -v -v -v -V --add all --postgres -pgv pg12
# Using the optional arguments
~/pg_monitor/timescaledb/custom/build_timescaledb.sh --org=lloydalbin ---pg_name=postgres

# For the second time, otherwise the postgres Dockerfile will get double patched.
~/pg_monitor/timescaledb/custom/build_timescaledb.sh --clean postgres --override_exit
```

### PostgreSQL Versions

The build_timescaledb.sh script has been updated to use PostgreSQL 12, but still works for PostgreSQL 11. To build PostgreSQL 11, just change the PG_VER variable to be pg11.

## Clean / Delete Repositories

If you wish to delete the repositories, you may do so manually or you can use the make command to clean up the postgres & timescaledb-docker repositories.

```bash
# Delete repositories
~/pg_monitor/timescaledb/custom/build_timescaledb.sh --clean
# Optional: Just Postgres
~/pg_monitor/timescaledb/custom/build_timescaledb.sh --clean postgres
# Optional: Just TimescaleDB
~/pg_monitor/timescaledb/custom/build_timescaledb.sh --clean timescaledb
```

## Cron

You can also have these auto-created via a cronjob on an hourly basis.

```cron
# With default arguments
* 0 * * * $HOME/pg_monitor/timescaledb/custom/build_timescaledb.sh
# With optional arguments
* 0 * * * $HOME/pg_monitor/timescaledb/custom/build_timescaledb.sh --org=lloydalbin --ts_name=timescaledb --pg_name=postgres
```