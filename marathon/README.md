# Update Marathon-UI from Marthon 1.4.5

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

