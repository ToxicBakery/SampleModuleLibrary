# Gradle Demonstration
This repository is a demonstration of Gradle functionality common in daily usage. The application demonstrates how to setup multiple build types and flavors for managing builds. The library demonstrates how to use Gradle for pushing Android Application Resources (AAR) to Maven.

---
# Application Variants
The sample application demonstrates use of Build Types combine with Flavors. This combination allows for full use of the Android build system to define flexible deployments. To quickly switch between variants, select the 'Build Variants' window tab which is typically in towards the bottom left in Android Studio on vertical bar just above the 'Terminal' tab.

From the variant window, select the build variant value next to the ```app``` module. This will reveal a drop down of the available variants. Once a variant is selected, previewing the ```main_activity.xml``` layout reflect the change in selected variant.

The variants are driven by the configuration of the ```build.gradle``` script in the module combined with the resources in the various build type folders.

---

# Library Setup to Deploy to Central (Sonatype)
To use this library you need to prepare a few things on your machine. 

## Create a PGP Signature
You will need to create a PGP Signature to sign your artifacts. An easy method of accomplishing this task is to use the free [GnuPG tool](https://www.gnupg.org/download/). For more detailed information on this step, please consider the [Central Guide](http://central.sonatype.org/pages/working-with-pgp-signatures.html) for assistance. Once installed, use the following to generate your key pair.

Create the key pair
```
gpg --gen-key
```

List the key pairs
```
gpg2 --list-keys
```

Distribute the public key
```
gpg2 --keyserver hkp://pool.sks-keyservers.net --send-keys <key id from list>
```

## Configuration File
To prevent exposing secrete information you will create a ```gradle.properties``` file in your user folder under the .gradle folder.

 * Windows
  * ```\Users\<user>\.gradle\gradle.properties```
 * Linux
  * ```/home/<user>/.gradle/gradle.properties```

For Windows, make sure slashes are escaped. For Linux, obviously use forward slashes. Additionally, make sure the path to your **SECRET** key is correct.
```
signing.keyId=<maven signing id>
signing.password=<signing password>
signing.secretKeyRingFile=C:\\development\\maven.keystore.gpg

sonatype_user=<your username id>
sonatype_pass=<your encrypted password
```

# Prepare Your Release
After you create your awesome Android Library, update the ```library/gradle.properties``` file as demonstrated in the library module. The information contained in this file will be used by Gradle to generate a POM file for identifying general information about your software.

# Register with Sonatype
Sonatype requires an account be registered with them to manage artifacts. It is important to do this before attempting to deploy artifacts to Central.

Register an account:<br>
http://central.sonatype.org/pages/ossrh-guide.html#create-a-ticket-with-sonatype

Create a ticket requesting upload access:<br>
https://issues.sonatype.org/secure/CreateIssue.jspa?issuetype=21&pid=10134

# Deploy
The last step is to deploy your artifact. The Maven plugin included by the Maven Deploy script provides an easy hook for accomplishing this. Simply run the ```uploadArchives``` task to push your artifact to Maven Central.
