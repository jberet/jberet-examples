If you want to run the provided WildFly server manually, run with this command:

```bash
$ mvn wildfly:run
```

And after the WildFly server is started, you can use the `setup-server.sh` to configure the WildFly server to use the JDBC repository for the `batch-jberet` subsystem of WildFly.

## Using the PostgreSQL database as the job repository.

Create the database first:

