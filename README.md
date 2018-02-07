# Demo Java Web App

Simple java project demos how to build a war file to be deployed on a Tomcat server.







## Initial Generation

The initial files and project structure was generated with the `mvn archetype:generate` command.  Note, you do not have to run the command it is just noted here for posterity.

``` sh
mvn archetype:generate \
  -DinteractiveMode=false \
  -DgroupId=com.domain \
  -DartifactId=demo \
  -DarchetypeArtifactId=maven-archetype-webapp
```

