If you want to run the provided WildFly server manually, run with this command:

```bash
$ mvn wildfly:run
```

And after the WildFly server is started, you can use the `setup-server.sh` to configure the WildFly server to use the JDBC repository for the `batch-jberet` subsystem of WildFly.

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

And after the WildFly server is started as described above, run the `setup-server-postgresql.sh` script to setup the datasource in WildFly server. Here is the output of the script:

```bash
$ ./setup-server-postgrsql.sh
+ JBOSS_HOME=target/wildfly
+ target/wildfly/bin/add-user.sh -u admin -p admin
Updated user 'admin' to file '/Users/weli/works/jberet-examples/deployment/target/wildfly/standalone/configuration/mgmt-users.properties'
+ target/wildfly/bin/jboss-cli.sh -c '--commands=xa-data-source add --name=batch_db --enabled=true --use-java-context=true --use-ccm=true --jndi-name=java:jboss/jsr352/batch_db --xa-datasource-properties={"URL"=>"jdbc:postgresql://localhost:5432/batch_db"} --driver-name=postgresql --password=123 --user-name=batch_user --same-rm-override=false --no-recovery=true, /subsystem=batch-jberet/jdbc-job-repository=batch_db:add(data-source=batch_db), /subsystem=batch-jberet/:write-attribute(name=default-job-repository,value=batch_db)'
{"outcome" => "success"}
{
    "outcome" => "success",
    "response-headers" => {
        "operation-requires-reload" => true,
        "process-state" => "reload-required"
    }
}
```

Here's the console output of from the server:


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