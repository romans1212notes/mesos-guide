# Create Basic Application
https://mesosphere.github.io/marathon/docs/application-basics.html

## Simple way is to copy and paste this json
```
{
    "id": "basic-0", 
    "cmd": "while [ true ] ; do echo 'Hello Marathon' ; sleep 5 ; done",
    "cpus": 0.1,
    "mem": 10.0,
    "instances": 1
}
```

# Build from Source
[release/1.4](https://github.com/mesosphere/marathon/tree/releases/1.4) documentation is better than master.
```
git clone https://github.com/mesosphere/marathon.git
cd marathon
# sbt clean package - package doesn't create the uber jar.
sbt assembly # create the uber jar
./bin/build-distribution # package Marathon as an executable JAR (optional): target/marathon-runnable.jar
```

# How Marathon UI Works


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

## Lessons Learned
* marathon-logo.png doesn't show well on Chrome (i.e. the background is not shown well, which is basically replaced with the white default background, and so white MARATHON foreground cannot be seen), but is good on Firefox.
* Debug the UI using Chrome developer tool, e.g. edit the dynamically generated HTML (e.g. remove class, attributes)
