# DHIS2 Core Application workflow
==

# Basic _everyday_ workflow overview

+ Write code
+ Run your build step that places the _artefact_ in a `build` folder (Webpack, gulp, scripts, etc) 
+ Run `mvn deploy`

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

## Deployment to maven

## Registering with DHIS2

### Registering the app as a dependency

### Registering the app as a module
