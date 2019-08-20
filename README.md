# Yaks Extension

This repository contains a sample project that shows how to quickly inject custom steps into a Yaks testing environment.

Custom steps are located in the [dev.yaks.testing.standard](/src/main/java/dev/yaks/testing/standard) package.
You can add your own steps, but **do not change the name of the package**.

## Initial Setup

Setup assumes that yaks is already installed in the current namespace on OpenShift.
Refer to the [Yaks documentation](https://github.com/jboss-fuse/yaks) for the installation instructions.

To check if Yaks is installed, you can run the following command:

```
oc get deployment yaks
```

Expected result is something like:
```
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
yaks      1         1         1            1           22m
``` 

Current Yaks release does not provide a way to dynamically load libraries, but
you can load your own by extending the base Yaks image.
This maven project creates a custom image on top of the Yaks one.

In order to use the custom image, you just need to tell yaks to use your own.
This can be done with a single command:

```
oc set env deployment/yaks TEST_BASE_IMAGE=yaks-extension:latest
```

## Adding your steps

First you need to add your steps in the [dev.yaks.testing.standard](/src/main/java/dev/yaks/testing/standard) package.
You can use as example the [MySteps](/src/main/java/dev/yaks/testing/standard/MySteps.java) class.

Once you've defined your steps, run the following command to install the image in the current namespace:

```
mvn clean install
```

This will start a OpenShift binary build that will produce the `yaks-extension:latest` library.

You can now create the feature file, for example the [my.feature](/my.feature) one,
and run it using:

```
yaks test my.feature
```

If you've defined your steps correctly, your code will be executed.
