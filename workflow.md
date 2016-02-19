# DHIS2 Core Application workflow
==

# Basic _everyday_ workflow overview
The above is however more steps than we as app developers generally have to deal with, a more simplified everyday workflow overview would look more like 

1. Write code
2. Run the build step 
3. Run `mvn deploy`

> The build step is a step in the process that places the _artefact_ in a folder named `build` in the root directory of the project. The contents of this folder go into the jar file that is published to Sonatype. (Webpack, gulp, scripts, etc)

#Initial setup

## Source control

### Creating the github repo
Create a new github repository for your application at the following location
https://github.com/organizations/dhis2/repositories/new

The recommended name for the application separated by dashes and suffixed with `-app` (E.g. `settings-app`, `maintenance-app`, `app-management-app`) 

### Using git
If you do not have one yet, run the following command in root directory of your application to initialize the git repository. 
```bash
git init
```

Create an initial commit of the source code
```bash
git commit -am "Initial commit"
```
> If you have files you do not want to go into the repository add them to a file called `.gitignore` in the root folder of the app.


To push your first batch of code to github you need to add the remote location of the repository (this is comparable with `bzr bind`)
```bash
git remote add origin https://github.com/dhis2/<name of your application>.git
```

Following this we can push the code to github.
```bash
git push -u origin master
```
> Following this on consecutive commands a `git push` should suffice

## Creating the pom.xml

## Building application

## Deployment to Sonatype
After creating the `pom.xml` and creating a build step for your app deploying to Sonatype is just a simple step.

```bash
mvn deploy
```
> This is assuming you configured your maven as defined in the Maven/Sonatype setup.

## Registering with DHIS2

### Registering the app as a dependency
To let DHIS2 know which applications to install when building the `dhis-web-apps` module we need to register our application as a dependency. We do this by specifying the name of our application on sonatype and the output directory of where to place the application.

The following xml represents the settings app.  
```xml
<artifactItem>
    <groupId>org.hisp.dhis</groupId>
    <artifactId>dhis-app-settings</artifactId>
    <version>${apps.version}</version>
    <overWrite>true</overWrite>
    <outputDirectory>${project.build.directory}/${project.artifactId}/dhis-web-settings</outputDirectory>
    <includes>**/*.*</includes>
</artifactItem>
```

### Registering the app as a module
