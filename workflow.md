# Basic _everyday_ workflow overview
The steps that we as app developers generally have to deal with to publish an application consist of a three step workflow: 

1. Write code
2. Run the build step 
3. Run `mvn clean deploy`

> The build step is a step in the process that places the _artefact_ in a folder named `build` in the root directory of the project. The contents of this folder get packed up into the jar file that is published to Sonatype. This build step can be anything you want (Webpack, gulp, scripts, etc) as long as it results into a `./build` folder with your files and the `manifest.webapp`.

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

We can use the `settings-app` as a basis to create our own `pom.xml` (You will find it [here](https://github.com/dhis2/settings-app/blob/c5206b81afc4f80207fb6ded71f6922fcc1b5191/pom.xml))

The following lines are the important ones to adjust to your own application:

+ 8 - Change `artifactId` to fit the correct name `dhis-app-<your-app-name>`
+ 9 - The `version` should correspond with the version of DHIS2 you wish to publish for. The version should always be a _snapshot_ repository (If you are publishing for 2.23 then your version should be 2.23-SNAPSHOT) that way the latest version of the app will always be downloaded.
+ 11 - Change `name` to reflect your apps name 
+ 12 - Change the description of the app to match your own app
+ 22-25 - Here you can change the developer details. (Add more `<developer>` blocks if you have multiple.
+ 36-40 - Change these source control urls to match the corresponding github repository
+ 44 - Here you can put the link you your github repository or the launchpad bug page.

## Building application

If you are using npm to build your application and have create a `build` script in your `package.json`, you can run.
```bash
npm run build
```
> If you are not using npm/webpack to build your project you can replace this with a simple script that copies your files or a `gulp` task.

## Deployment to Sonatype
> When deploying the applications to Sonatype, they do not get _installed_ like other DHIS2 applications, therefore the server does not update the `manifest.webapp` file with the correct base url. We will therefore set the `activities.dhis.href` property in the manifest to `..` instead of `*`.

After creating the `pom.xml` and creating a build step for your app deploying to Sonatype is just a simple step.

```bash
mvn clean deploy
```
> This is assuming you configured your maven as defined in the [Maven/Sonatype setup](https://github.com/dhis2/dhis2-apps-docs/blob/master/maven_sonatype_setup.md).

The app is now deployed to the Maven repository and ready to be _installed_ into DHIS2.

## Registering with DHIS2

### Registering the app as a dependency
To let DHIS2 know which applications to install when building the `dhis-web-apps` module we need to register our application as a dependency. We do this by specifying the name of our application on SonaType and the output directory of where to place the application.

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

We need to add this piece of xml to the `pom.xml` file of of the `dhis-web-apps` module on Launchpad. 

> Note that the artifact idea is prefixed `dhis-app-` while the _outputDirectory_ is prefixed with `dhis-web-`

### Registering the app as a module

After registering the app as a dependency we need to make it known to Struts as a module. In the same `dhis-web-apps` project, we go to
[struts.xml](http://bazaar.launchpad.net/~dhis2-devs-core/dhis2/trunk/view/head:/dhis-2/dhis-web/dhis-web-apps/src/main/resources/struts.xml#L67) that is located in `src/main/resources` to register our app with the follow piece of XML.

> Note that the `dhis-web-<app-name> corresponds with the _outputDirectory_ as defined in the previous step. The same goes for the _namespace_ property.

> If your app starts at a different location than `index.html` in the root of the application, you also need to change the `action.name` property in the snippet below.

```xml
<package name="dhis-web-settings" extends="dhis-web-commons" namespace="/dhis-web-settings">
    <action name="index" class="org.hisp.dhis.commons.action.NoAction">
        <result name="success" type="redirect">#</result>
    </action>
</package>
```
