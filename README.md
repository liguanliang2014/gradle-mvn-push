gradle-mvn-push
===============

See this blog post for more context on this 'library': [http://chris.banes.me/2013/08/27/pushing-aars-to-maven-central/](http://chris.banes.me/2013/08/27/pushing-aars-to-maven-central/).


## Usage

### 1. Have a working Gradle build
This is upto you.

### 2. Update your home gradle.properties

This will include the username and password to upload to the Maven server and so that they are kept local on your machine. The location defaults to `USER_HOME/.gradle/gradle.properties`.

It may also include your signing key id, password, and secret key ring file (for signed uploads).  Signing is only necessary if you're putting release builds of your project on maven central.

```properties
# your maven config
MAVEN_USERNAME=<your maven username>
MAVEN_PASSWORD=<your maven password>

# your jcenter config
JCENTER_USERNAME=<your jcenter username>
JCENTER_PASSWORD=<your jcenter apikey>

# your gpg key config
signing.keyId=ABCDEF12
signing.password=n1c3try
signing.secretKeyRingFile=~/.gnupg/secring.gpg
```

### 3. Create project root gradle.properties
You may already have this file, in which case just edit the original. This file should contain the POM values which are common to all of your sub-project (if you have any). For instance, here's [ActionBar-PullToRefresh's](https://github.com/chrisbanes/ActionBar-PullToRefresh):

```properties
VERSION_NAME=0.9.2-SNAPSHOT
VERSION_CODE=92
GROUP=com.github.chrisbanes.actionbarpulltorefresh

POM_DESCRIPTION=A modern implementation of the pull-to-refresh for Android
POM_URL=https://github.com/chrisbanes/ActionBar-PullToRefresh
POM_SCM_URL=https://github.com/chrisbanes/ActionBar-PullToRefresh
POM_SCM_CONNECTION=scm:git@github.com:chrisbanes/ActionBar-PullToRefresh.git
POM_SCM_DEV_CONNECTION=scm:git@github.com:chrisbanes/ActionBar-PullToRefresh.git
POM_LICENCE_NAME=The Apache Software License, Version 2.0
POM_LICENCE_URL=http://www.apache.org/licenses/LICENSE-2.0.txt
POM_LICENCE_DIST=repo
POM_DEVELOPER_ID=chrisbanes
POM_DEVELOPER_NAME=Chris Banes
POM_DEVELOPER_EMAIL=itlgl@outlook.com

# generate aar in local
LOCAL_REPOSITORY_URL=file://D:/temp/

# upload to Jcenter
JCENTER_RELEASE_REPOSITORY_URL=https://api.bintray.com/maven/itlgl/maven/iosdialog/;publish=1
JCENTER_SNAPSHOT_REPOSITORY_URL=https://api.bintray.com/maven/itlgl/maven/iosdialog/;publish=1
```

The `VERSION_NAME` value is important. If it contains the keyword `SNAPSHOT` then the build will upload to the snapshot server, if not then to the release server.

### 4. Create gradle.properties in each sub-project
The values in this file are specific to the sub-project (and override those in the root `gradle.properties`). In this example, this is just the name, artifactId and packaging type:

```properties
POM_NAME=ActionBar-PullToRefresh Library
POM_ARTIFACT_ID=library
POM_PACKAGING=aar
```

### 5. Call the script from each sub-modules build.gradle

Add the following at the end of each `build.gradle` that you wish to upload:

#### For Android Project
```groovy
apply from: 'https://raw.github.com/itlgl/gradle-mvn-push/v1.0.2/gradle-mvn-push-android.gradle'
```

#### For JAVA Project
```groovy
apply from: 'https://raw.github.com/itlgl/gradle-mvn-push/v1.0.2/gradle-mvn-push-java.gradle'
```

### 6. javadoc "编码GBK的不可映射字符" error

Add properties `JAVADOC_FILE_ENCODING` in gradle.properties file:
```groovy
JAVADOC_FILE_ENCODING=<your java source file encoding,default utf-8>
```

### 7. Upload to Maven/Jcenter/Local

upload to maven
```
gradle uploadToMaven
```

upload to Jcenter
```
gradle uploadToJcenter
```

upload to Local
```
gradle uploadToLocal
```

## Thanks

[chrisbanes gradle-mvn-push](https://github.com/chrisbanes/gradle-mvn-push)

## License

    Copyright 2013 Chris Banes

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
