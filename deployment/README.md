If you want to run the provided WildFly server manually, run with this command:

```bash
$ mvn wildfly:dev
```

## Using the PostgreSQL database as the job repository.

To use the PostgreSQL database as the backend job repository, you need to create the database first. Here are the commands to do so:

```sql
postgres=# create user batch_user password '123';
CREATE ROLE
```

```sql
postgres=# create database batch_db owner batch_user;
CREATE DATABASE
```

### Using Docker or Podman
```bash
podman run -it -e POSTGRES_PASSWORD=123 -e POSTGRES_USER=batch_user -e POSTGRES_DB=batch_db -p 5432:5432 postgres
```

Then you need to enable the prostgres profile. This can be done with `-Ppostgres` or `-Dpostgres`.

```bash
$ mvn wildfly:dev -Ppostgres
```

or:

```bash
$ mvn wildfly:dev -Ppostgres-ds
```

Here's the console output of from the server:

If you'd like to use the `postgresql-datasource` layer instead of the `postgresql-driver` layer, this can be enabled with
the `-Ppostgres-ds` or `-Dpostgres.ds` arguments. The layer being used simply depends on if you want to use CLI to
create the data source or have the layer create it for you.

```bash
$ mvn wildfly:dev -Ppostgres-ds
```

or:

```bash
$ mvn wildfly:dev -Dpostgres.ds
```

```txt
01:28:43,076 INFO  [org.jboss.as.connector.subsystems.datasources] (MSC service thread 1-8) WFLYJCA0001: Bound data source [java:jboss/jsr352/batch_db]
01:28:43,566 INFO  [org.jberet] (ServerService Thread Pool -- 77) JBERET000021: About to initialize batch job repository with ddl-file: sql/jberet-postgresql.ddl for database PostgreSQL
```

Now the tables are created in the database:

```txt
batch_db=# \dt
                 List of relations
 Schema |        Name         | Type  |   Owner
--------+---------------------+-------+------------
 public | job_execution       | table | batch_user
 public | job_instance        | table | batch_user
 public | partition_execution | table | batch_user
 public | step_execution      | table | batch_user
(4 rows)
```