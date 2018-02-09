# Demo Java Web App

Simple java project demos how to build a war file to be deployed on a Tomcat server.

## Build

The build script uses `mvn package` to produce a demo.war file and then bundles it with a Docker image that runs Tomcat.  Usage:

```sh
bin/build
```

## What happened

* mvn package was ran and the `target/demo.war` was moved into `pkg/demo.war`
* a docker image was built which copied the `pkg/demo.war` to `/usr/local/tomcat/webapps/demo.war`. Check out the [Dockerfile](Dockerfile) for details.

Here's an example of some things to check after running the build script:

```
$ ls pkg/demo.war
pkg/demo.war
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
demo-java           latest              88092dfb7325        6 minutes ago       591MB
tomcat              8.5                 a92c139758db        2 weeks ago         558MB
$
```

## Run

To run and test that Tomcat is serving the the war file:

```
docker run --rm -p 8080:8080 demo-java
```

Then you can hit the the [HOSTNAME]:8080/demo/Hello and to verify that Tomcat is servering the demo.war file.  You should see an html page that says "Hello World"

## Usage with UFO

The ufo branch of this project provides an additional demo that takes the war artifact, builds a Docker image and deploys it to ECS.  For details please check out that branch: [ufo](https://github.com/tongueroo/demo-java/tree/ufo). For more details on ufo check out the [official ufo docs](http://ufoships.com/).

## Initial Generation

Here are some notes on the initial generation of the project. The initial files and project structure was generated with the `mvn archetype:generate` command.  Note, you do not have to run the command it is just noted here for posterity.  More info: [Creating a webapp](https://maven.apache.org/plugins-archives/maven-archetype-plugin-1.0-alpha-7/examples/webapp.html) and [Introduction to the Standard Directory Layout](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html).

Change were made like adding a simple [Hello.java](src/main/java/Hello.java) Serlvet class.

The original command was:

``` sh
mvn archetype:generate \
  -DinteractiveMode=false \
  -DgroupId=com.domain \
  -DartifactId=demo \
  -DarchetypeArtifactId=maven-archetype-webapp
```

## Dependencies

* docker: `brew install docker`
* maven: `brew install maven`
