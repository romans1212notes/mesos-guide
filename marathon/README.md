# Basic
## Command Line flags
https://github.com/mesosphere/marathon/blob/master/docs/docs/command-line-flags.md

## Create Basic Application
https://mesosphere.github.io/marathon/docs/application-basics.html

Simple way is to copy and paste this json
```
{
    "id": "basic-0", 
    "cmd": "while [ true ] ; do echo 'Hello Marathon' ; sleep 5 ; done",
    "cpus": 0.1,
    "mem": 10.0,
    "instances": 1
}
```

## Build from Source
[release/1.4](https://github.com/mesosphere/marathon/tree/releases/1.4) documentation is better than master.
```
git clone https://github.com/mesosphere/marathon.git
cd marathon
git checkout releases/1.4
# sbt clean package - package doesn't create the uber jar.
sbt assembly # create the uber jar
./bin/build-distribution # package Marathon as an executable JAR (optional): target/marathon-runnable.jar
```

## How Marathon UI Works
Marathon-ui is created as a separate package, and it is an dependency of marathon. After building with "sbt package", the package is downloaded at:
```.coursier/cache/v1/https/downloads.mesosphere.com/maven/mesosphere/marathon/ui/1.2.0/ui-1.2.0.jar```

The content of it can be found by:
```
# jar tf ~/.coursier/cache/v1/https/downloads.mesosphere.com/maven/mesosphere/marathon/ui/1.2.0/ui-1.2.0.jar
META-INF/MANIFEST.MF
META-INF/resources/webjars/ui/*  # python -m SimpleHTTPServer 8080 in this directory can server the UI.
META-INF/maven/mesosphere.marathon/ui/pom.xml
META-INF/maven/mesosphere.marathon/ui/pom.properties

# unzip the file
# jar xf ~/.coursier/cache/v1/https/downloads.mesosphere.com/maven/mesosphere/marathon/ui/1.2.0/ui-1.2.0.jar
```


# Customize Marathon-UI from Marthon 1.4.5

## Extract jar file
```
marathon/target/scala-2.11$ jar xf marathon-assembly-1.4.5.jar
```

## Update marathon-assembly
Edit files that need to be updated, e.g. index.html and main.js, then
```
marathon/target/scala-2.11$ jar uf marathon-assembly-1.4.5.jar META-INF/resources/webjars/ui/index.html
marathon/target/scala-2.11$ jar uf marathon-assembly-1.4.5.jar META-INF/resources/webjars/ui/main.js
```

## Create Runnable
```
marathon $ bin/build-distribution
Concatenating
  bin/marathon-framework
  target/scala-2.11/marathon-assembly-1.4.5.jar
to form
  target/marathon-runnable.jar
```
Which basically caculate the shasum of marathon-assembly-1.4.5.jar and put it in bin/marathon-framework, and then concatenate bin/marathon-framework and marathon-assembly-1.4.5.jar together.

## Run
```
(sudo) marathon/target/marathon-runnable.jar.v4 --master=localhost:5050
```

## Lessons Learned
* marathon-logo.png doesn't show well on Chrome (i.e. the background is not shown well, which is basically replaced with the white default background, and so white MARATHON foreground cannot be seen), but is good on Firefox.
* Debug the UI using Chrome developer tool, e.g. edit the dynamically generated HTML (e.g. remove class, attributes)

## Related Work
https://github.com/mesosphere/marathon/issues/3884
