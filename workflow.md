# Basic _everyday_ workflow overview
The steps that we as app developers generally have to deal with to publish an application consist of a three step workflow: 

1. Write code
2. Run the build step 
3. Run `mvn deploy`

> The build step is a step in the process that places the _artefact_ in a folder named `build` in the root directory of the project. The contents of this folder go into the jar file that is published to Sonatype. (Webpack, gulp, scripts, etc)

## Source control
The source code for the applications is kept in github repositories. Lets first create a repository for our application and put our code on github.

### Creating the github repo
Create a new github repository for your application at the following location
https://github.com/organizations/dhis2/repositories/new

The recommended name for the application separated by dashes and suffixed with `-app` (E.g. `settings-app`, `maintenance-app`, `app-management-app`) 

### Using git
If you do yet not use git for your app, run the following command in root directory of your application to initialize the git repository. 
```bash
git init
```

We will need to create an initial commit of the source code to be able to push it to GitHub
```bash
git commit -am "Initial commit"
```
> If you have files you do not want to go into the repository add them to a file called `.gitignore` in the root folder of the app.


To push your first batch of code to GitHub you need to add the remote location of the repository (this is comparable with `bzr bind`)
```bash
git remote add origin https://github.com/dhis2/<name of your application>.git
```

Following this we can push the code to GitHub.
```bash
git push -u origin master
```
> For pushing consecutive commits a `git push` should suffice (After running the above command once)

## Creating the pom.xml
To be able to register our app with Sonatype we have to create a `pom.xml` that describes our application. This `pom.xml` file will be used to register our app on Sonatype so we can use Maven to include it in the DHIS2 build process.

We can use the `settings-app` as a basis to create our own `pom.xml` (You will find it [here](github.com/dhis2/settings-app/blob/c5206b81afc4f80207fb6ded71f6922fcc1b5191/pom.xml)

The following lines are the important ones to adjust to your own application:
8 
11
12 
22-25
36-40
44

## Building application

If you are using npm to build your application and have create a `build` script in your `package.json`, you can run.
```bash
npm run build
```

## Deployment to Sonatype
After creating the `pom.xml` and creating a build step for your app deploying to Sonatype is just a simple step.

```bash
mvn deploy
```
> This is assuming you configured your maven as defined in the Maven/Sonatype setup.

The app is now deployed to the Maven repository and ready to be _installed_ into DHIS2.

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
