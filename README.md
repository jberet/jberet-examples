## JBeret Examples

The JBeret Examples aims to provide examples of usages of the JBeret project to run different kinds of jobs. The project has two modules:

- standalone
- deployment

The `standalone` example contains jobs that run in the JBeret SE environment. The Weld container is used for running the jobs under this environment. On the other hand, the `deployment` example contains jobs that run in WildFly server. The `wildfly-maven-plugin` is used to provision a WildFly server for usage.

## Usage

- Run `mvn install` to run all the examples.