# Maven/Sonatype setup

## Install gnupg
To sign the certificate for the jar file we use `gnupg`. If you do not have this available yet you can install it using the following commands.

On ubuntu
```bash
sudo apt-get install gnupg
```

On OSX (using homebrew)
```bash
brew install gnupg
```


## Generate a key pair with
We also need a key to do the signing. Lets generate one using:
```bash
gpg --gen-key
```

> Go with the defaults, this key is used for artefact signing

## Maven setup 
Finally we need to identify ourselves to the Sonatype repository. We can do this using a Maven settings file. Place a `settings.xml` file in `~/.m2/settings.xml`.

This file should have the following contents and the correct username/password filled in.
 
```xml 
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <servers>
    <server>
      <id>ossrh</id>
      <username>xxxx</username>
      <password>xxxx</password>
    </server>
  </servers>
</settings>
```
