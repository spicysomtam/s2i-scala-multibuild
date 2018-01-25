# Scala s2i multi build for OpenShift

This is a s2i image builder for scala that supports multiple sbt projects that are linked together.
For example within your source code you have multiple directories, each containing an sbt build.
Some of the directory builds are called/included in other directories. That is a dir will
use common code from other directories.

An env var is passed to the builder image to specify which subdir builds should be performed, with the target for each subdir. Ideally the last subdir to be built will generate the final app to run, and this should ideally have target `stage` to ensure a java compatible run script/packaging is created:

SBT_BUILDS="dir1/clean dir2/publishLocal dir/stage" 

[s2i is here.](https://github.com/openshift/source-to-image)
The resulting image can be run using [Docker](http://docker.io) for testing, but the ultimate aim is to use an openshift build config to build it.

s2i takes a build image, injects code into the builder image, and then creates an output image based on the builder image, with the application ready to run.
s2i can also create another image with the build code/tools removed and just containing the application.

The build process uses a builder volume to do the build, which is `/opt/build` within the container, to ensure all the downloaded sbt components and source code are not included within the final image (and don't create a huge image). When testing on the command line, you can use the `s2i build -v` arg for this; within an openshift build config you can use a volume and have a permanently defined build volume to speed up building (eg all the sbt downloaded components are not re downloaded at each build). The -v arg is similar to
`docker build -v`.

# Versions

Scala versions currently provided are:
* SBT latest (1.1.0?)

Java versions currently provided are:
* Java 1.8.0

CentOS versions currently supported are:
* CentOS 7

# Installation

To create the scala s2i builder image:

```
$ git clone https://github.com/spicysomtam/s2i-scala-multibuild.git
$ cd s2i-scala-multibuild/sbt-java-8
$ make build
```

# Usage

```
$ cd <code-base>
$ mkdir /var/tmp/s2i
$ s2i build --loglevel 3 -v /var/tmp/s2i:/opt/build --env SBT_BUILDS="dir1/clean dir2/publishLocal dir/stage" --env DEBUG=true . spicysomtam/s2i-scala-multibuild:java8 foo 2>&1|tee b
$ docker run -d foo
```

Test
---------------------
This repository also provides a [S2I](https://github.com/openshift/source-to-image) test framework,
which launches tests to check functionality of a simple Scala application built on top of the sti-Scala image.

Users can choose between testing a Scala test application based on a RHEL or CentOS image.

```
$ cd sti-scala/sbt-0.13-java-8
$ make test
```
