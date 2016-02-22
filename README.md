# DHIS2 Core apps docs

This repo contains a set of instructions on how to set up your machine/repositories to create DHIS2 applications using the DHIS2 Sonatype maven repository to include the application in the DHIS2 build step.

## Core App Architecture overview

1. The source code for the app is located in github repos. 
2. Apps are _built_ (using npm/webpack/gulp etc.).
3. Apps are deployed to the DHIS2 Sonatype maven snapshot repository as jar files.
4. Apps are defined as maven dependencies in the core code in `dhis-web-apps` module (on launchpad).
5. In `dhis-web-apps` in core, maven unpacks the jar file and copies it to target directory, where it becomes part of the war file.

## Maven Sonatype setup
To be able to deploy to Sonatype we have to do some setup, the [Maven Sonatype setup guide](https://github.com/dhis2/dhis2-apps-docs/blob/master/maven_sonatype_setup.md) will guide you through these steps. This is just a one time process.

## App workflow setup
The process for developing creating apps is a repeating process for each separate app you create. The [Workflow guide](https://github.com/dhis2/dhis2-apps-docs/blob/master/workflow.md) will guide you through these steps.

## Daily workflow steps
1. Write code/tests
2. Run build step (`npm run build`)
3. `mvn deploy`

