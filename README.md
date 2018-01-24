# Scala s2i multi build for OpenShift

This repository contains the source for building Scala applications as a reproducible Docker image using
[source-to-image](https://github.com/openshift/source-to-image).
The resulting image can be run using [Docker](http://docker.io).

This version supports multiple build sub directories; the idea is you might want to include common code and thus you need to build the common code and then your application.

# Versions

Scala versions currently provided are:
* SBT latest (1.1.0?)

Java versions currently provided are:
* Java 1.8.0

CentOS versions currently supported are:
* CentOS 7


# Installation

*  **CentOS based image**

    To build a Scala image from scratch run:

    ```
    $ git clone https://github.com/spicysomtam/s2i-scala-multibuild.git
    $ cd s2i-scala-multibuild/sbt-java-8
    $ make build
    ```


# Usage
To build a simple [Scala-sample-app](https://github.com/pat2man/play-originv3-test) application
using standalone [STI](https://github.com/openshift/source-to-image) and then run the
resulting image with [Docker](http://docker.io) execute:

*  **For CentOS based image**
    ```
    $ sti build https://github.com/ticketfly/sti-scala.git --context-dir=sbt-0.13-java-8/test/test-app/ ticketfly/scala-0.13-java-8-centos7 scala-test-app
    $ docker run -p 9000:9000 scala-test-app
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
