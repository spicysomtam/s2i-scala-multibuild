# Scala s2i multi build for OpenShift

This is a s2i image builder for scala that supports multiple sbt projects that are linked together.
For example within your source code you have multiple directorys, each containing an sbt build.
Some of the directory build components that are included in other directories. For example one dir will
use common code from other directories.

[s2i is here.](https://github.com/openshift/source-to-image).
The resulting image can be run using [Docker](http://docker.io).

s2i takes a build image, injects code into the builder image, and then creates an output image based on the builder image, with the application ready to run.
s2i can also create another image with the build code/tools removed and just containing the application.

This repo is for a scala builder image.

# Versions

Scala versions currently provided are:
* SBT latest (1.1.0?)

Java versions currently provided are:
* Java 1.8.0

CentOS versions currently supported are:
* CentOS 7

Docker ce client:

# Docker client

I have included the docker-ce client. This will allow you to use `docker build`, etc, and to interact with the underlying dockerd.

To use this, you must use `-v /var/run/docker.sock:/var/run/docker.sock` with the build command.

The docker client will do all the normal checks; you must run as root or be part of the docker group in the container.

# Installation

    To create the scala s2i builder image:

    ```
    $ git clone https://github.com/spicysomtam/s2i-scala-multibuild.git
    $ cd s2i-scala-multibuild/sbt-java-8
    $ make build
    ```
# Usage
To build a simple [Scala-sample-app](https://github.com/pat2man/play-originv3-test) application
using standalone [STI](https://github.com/openshift/source-to-image) and then run the
resulting image with [Docker](http://docker.io) execute:

    ```
    $ cd <code-base>
    $ sti build . spicysomtam/s2i-scala-multibuild:java8 my-image
    $ docker run my-image
    ```

Test
---------------------
This repository also provides a [S2I](https://github.com/openshift/source-to-image) test framework,
which launches tests to check functionality of a simple Scala application built on top of the sti-Scala image.

Users can choose between testing a Scala test application based on a RHEL or CentOS image.


*  **CentOS based image**

    ```
    $ cd sti-scala/sbt-0.13-java-8
    $ make test
    ```
