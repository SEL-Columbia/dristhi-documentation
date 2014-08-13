---
layout: page
title: "Dristhi Developer Machine Setup"
---

# Dristhi Developer Machine Setup

**Note:** This document is work in progress, please update this based on your setup experience

For Mac OS X 10.9.2 and later, make sure that [Homebrew][1] is installed, and use `brew install [pkg]`. To install a specific version of the [pkg], use `brew install [pkg] --version[version]`  
For Ubuntu/Debian use `sudo apt-get install [pkg]`  
For Red Hat/Fedora/CentOS use `sudo yum install [pkg]`

## Tools and Frameworks Setup

1. Apache Maven
    1. Install Apache Maven 3.0 using an appropriate package manager with package name `homebrew/versions/maven30`. We have had issues with newer versions of maven, so it is advisable to install 3.0
    2. Create/Update settings.xml file[^1] at &lt;maven directory&gt; (&lt;user_home_directory&gt;/.m2 on Mac OS X)
    3. Increase maven permgen size by creating an environment variable called MAVEN_OPTS with value "-XX:MaxPermSize=256m -Xmx512m". On Mac OS X this can be done by adding ```export MAVEN_OPTS="-XX:MaxPermSize=256m -Xmx512m"``` to bash_rc or bash_profile
2. Android SDK
    1. Download and install [Android SDK][2]. Alternatively you can install it using appropriate package manager with package name `android-sdk`
    2. Launch Android SDK Manager by using *android* command. Install the below mentioned components
        1. Tools -> Android SDK Tools
        2. Tools -> Android SDK Platform Tools
        3. Android 4.1.2 (API 16) -> SDK Platform
        4. Android 4.1.2 (API 16) -> ARM EABI v7a System Image
    3. Create an environment variable ANDROID_HOME pointing to the SDK root path
3. JDK Setup
    1. Install JDK 1.6 and 1.7 from [Oracle website][3]
    2. Create an environment variable JAVA_HOME which points to home directory of JDK 1.7. On Mac OS X, this may be located at /Library/Java/JavaVirtualMachines/jdk1.7.0_09.jdk/Contents/Home. But we have found the path to vary across machines, so please find the location for your machine and use that
4. Node modules setup. **Note:** Some of these commands may require sudo permissions.
    1. Install [NodeJS][4] using an appropriate package manager with package name `node` and version `0.10.17`
    2. Install [npm][5] using an appropriate package manager with package name `npm` and version `1.3.8`
    3. Install [PhantomJS][8] using an appropriate package manager with package name `phantomjs` and version `1.8.2`. **Note:** We have faced issues when running JS tests using Karma and PhantomJS due to conflicting versions of these being installed. The solution is to not have global installations (-g option), but it will require some development effort to get tests running with local installations.
    4. Install [Handlebars][6] using  `npm install -g handlebars@1.0.12`
    5. Install [Karma][7] using `npm install -g karma@0.10.1`
    6. Install [Grunt Command Line Tool][12] using `npm install grunt-cli@0.1.9`
    7. Install [CORS Proxy][17] using `npm install corsproxy@0.2.14`
5. Install [ActiveMQ][9] using an appropriate package manager with package name `activemq` and version `5.7.0`
    1. ActiveMQ needs to be started for Dristhi server to build, so follow the instructions at the end of the installation to start it the way you prefer
    2. ActiveMQ by default has only 1MB memory limit. This is usually not sufficient. Increase ActiveMQ memory by updating ActiveMQ configuration[^2]. ActiveMQ configuration can be found at &lt;activemq_install_location&gt;/conf/activemq.xml
6. Install [PostgreSQL][10] using an appropriate package manager with package name `postgresql` and version `9.2.1`
    1. PostgreSQL needs to be started for Dristhi server to build, so follow the instructions at the end of the installation to start it the way you prefer
    2. Create a user called 'postgres' by using `createuser -P postgres`. Set the password as 'password'. This is required for building server
    3. Login to database using `psql -U postgres`. Create a database called 'drishti' using `CREATE DATABASE drishti`
7. Install [CouchDB][11] using an appropriate package manager with package name `couchdb` and version `1.2.0`
8. Install [Git][16] using an appropriate package manager with package name `git`


## Dristhi Server Build
1. Clone Dristhi Server git repository from https://github.com/SEL-Columbia/dristhi using `git clone https://github.com/SEL-Columbia/dristhi`
2. Run `mvn clean install` from the root directory of Dristhi Server
3. To run server locally run
    1. Run `mvn clean install` from the root directory of Dristhi Server
    2. Navigate to &lt;server_root&gt;/drishti-reporting directory and run `mvn jetty:run `
    3. Navigate to &lt;server_root&gt;/drishti-web directory and run `mvn jetty:run`
    4. This can be simplified by creating an alias, `alias web='(cd <server_root>; cd drishti-reporting && mvn jetty:run &; cd ../drishti-web && mvn jetty:run)'`


## Dristhi App Build

**Note:** Dristhi App build depends on Server build, so please complete that before building app

1. Clone Dristhi app git repository from https://github.com/SEL-Columbia/drishti-app using `git clone https://github.com/SEL-Columbia/drishti-app`
2. Android Emulator
    1. **Default Emulator:** Create, for the first time, and start an Android Virtual Device by using `android avd` with API level 16 and screen resolution 1024 X 600
    2. **Genymotion**: Alternatively, you can also install [Genymotion][13] Android Emulator which is much faster than the default Android emulator.
        1. Install [Genymotion][13] emulator
        2. Install [Virtual Box][14]
        3. Follow [ARM Translation][15] instructions to get Dristhi app working on Genymotion
        4. Start Genymotion emulator and create a Virtual device with API level 16 and screen resolution 1024 X 600
3. Run `mvn clean install` from the root directory of Dristhi app


## Dristhi Dashboard Build

1. Clone Dristhi Dashboard git repository from https://github.com/SEL-Columbia/drishti-site using `git clone https://github.com/SEL-Columbia/drishti-site`
2. Run `npm install`
3. Run `bower install`
4. Run `grunt`. This will build the code (jshint verification, unit tests, concatenation, minification, etc) and generate runnable web app in `dist` directory
5. To run dashboard locally
    1. Run Dristhi Server locally by following the steps mentioned above
    2. In &lt;dashboard_root&gt;/app/scripts/app.js comment the section of constants that have prod URLs (https://smartregistries.org) and uncomment the section which has local URLs (http://localhost:9292/localhost:9980)
    3. Run `corsproxy`
    4. Run `grunt server` from &lt;dashboard_root&gt;

## Troubleshooting 

1. `The Genymotion virtual device could not obtain an IP address.`
     Make sure that a host only network is configured on your virtual box with DHCP server enabled. To ensure this, go to the virtual box's preferences. 
     On the network tab, go to the 'Host-only networks' tab and create a host only network if it does not exist. If it does, edit this network and go to the DHCP Server tab and check the "Enable server" checkbox. Provide all the necessary fields here. Now restart the genymotion virtual device.

[1]: http://brew.sh/
[2]: https://developer.android.com/sdk/index.html
[3]: http://www.oracle.com/technetwork/java/javase/downloads/index.html
[4]: http://nodejs.org/
[5]: https://npmjs.org/
[6]: http://handlebarsjs.com/
[7]: http://karma-runner.github.io/0.10/index.html
[8]: http://phantomjs.org/
[9]: http://activemq.apache.org/
[10]: http://www.postgresql.org/
[11]: https://couchdb.apache.org/
[12]: http://gruntjs.com/
[13]: http://www.genymotion.com/
[14]: https://duckduckgo.com/?q=virtualbox
[15]: http://forum.xda-developers.com/showthread.php?t=2528952
[16]: http://git-scm.com/
[17]: https://github.com/gr2m/CORS-Proxy

[^1]:
```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
 http://maven.apache.org/xsd/settings-1.0.0.xsd">
 <profiles>
   <profile>
     <id>android</id>
     <properties>
       <android.sdk.path>[path_to_android_sdk]</android.sdk.path>
       <drishti.updatePolicy>never</drishti.updatePolicy>
     </properties>
   </profile>
 </profiles>
 <activeProfiles>
   <activeProfile>android</activeProfile>
 </activeProfiles>
</settings>
```

[^2]:
```xml
<policyEntry queue=">" producerFlowControl="true" memoryLimit="500mb">
</policyEntry>
```
