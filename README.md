How to configure Artifactory plugin development
-----------------------------------------------

- Go to the `gradle.properties` file and set the `artifactoryVersion` to the version you are using
- Go to `gradle.properties` and paste in your download link for Artifactory
- Link any plugins to be worked on in the `etc/plugins` directory (see readme there), or
- If you are using the [`artifactory-user-plugins`](https://github.com/JFrogDev/artifactory-user-plugins)
  repository, paste that path into `gradle.properties`

All of the configuration happens with Gradle tasks:

To set up your IDE, type: `./gradlew idea` (or `./gradlew eclipse`)  
To download and configure Artifactory: `./gradlew prepareArtPro`  
To start Artifactory: `./gradlew startArtPro`

If you are using the [`artifactory-user-plugins`](https://github.com/JFrogDev/artifactory-user-plugins) repository:  
To link a plugin: `./gradlew workOnPlugin -DpluginName=plugin/name`  
To unlink a plugin: `./gradlew stopWorkOnPlugin -DpluginName=plugin/name`  
To unlink all plugins: `./gradlew stopWorkOnPlugin -DpluginName=all`

Then you can run the tests present in
`artifactory-user-plugins-devenv/src/test/groovy/org/jfrog/plugindev/test` using
your IDE.

To stop Artifactory: `./gradlew stopArtPro`  
To restart Artifactory: `./gradlew restartArtPro`  
To erase all Artifactory storages: `./gradlew cleanArtPro`

Artifactory will be configured with the Artifactory Pro license present in the
`etc` folder, a representative set of repositories, and will poll the `plugins`
directory for updates every 10 seconds.

Logging can be printed to the logs from within the plugin (with `log.warn`) and
viewed by tailing the `artifactory.log` file, as user plugins are compiled at
runtime. A basic example would be:

```java
storage {
        beforeMove{item, targetRepoPath, properties ->
                 log.warn "triggered !";
        }
}
```

(For checking whether the `beforeMove` event was triggered)
