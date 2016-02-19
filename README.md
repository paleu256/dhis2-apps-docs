# dhis2-apps-docs

# Core App Architecture overview

1. Apps are developed in github repos. 
2. Apps are _built_ (using npm).
3. Apps are deployed to our Sonatype maven snapshot repository as jar files.
4. Apps are defined as maven dependencies in the core code in `dhis-web-apps` module (on launchpad).
5. In `dhis-web-apps` in core, maven unpacks the jar file and copies it to target directory, where it becomes part of the war file.

# Maven Sonatype setup
To be able to deploy to Sonatype we have to do some setup, the Maven Sonatype setup guide will guide you through these steps. This is just a one time process.

# App workflow
The process for developing creating apps is a repeating process for each separate app you create. The Workflow guide will guide you through these steps.
