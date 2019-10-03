# Gradle hacks
Some useful code snippets created for some Gradle projects, most of them uses Kotlin DSL.

## Dependency management

### Dependencies from custom Nexus Artifactory
```kotlin
maven {
    credentials {
        username = Props.nexusUser.getOrDefault()
        password = Props.nexusPassword.getOrDefault()
    }
    url = URI("https://repo.ai/repository/maven-releases/")
}
```

## Testing

### Jacoco
Jacoco does not have direct support for the multimodule projects in Gradle, 
following extension function will include all subprojects to the reports.
```kotlin
/**
 * Configures current [JacocoReportBase] in a way that it includes subprojects to its reports
 */
fun JacocoReportBase.includeSubprojects() {
    executionData.setFrom(fileTree(project.rootDir.absolutePath).include("**/build/jacoco/*.exec"))

    subprojects
        .forEach {
            this.sourceSets(it.sourceSets["main"])
            this.dependsOn(it.tasks["test"])
        }
}
```
Usage is then following:
```kotlin
jacocoTestReport {
    includeSubprojects()

    reports {
        csv.isEnabled = false
        xml.isEnabled = true
        xml.destination = file(jacocoReportDir)
    }
}
```

## Deployment
All examples were taken from the Blindspot's ktoolz library.
To the Bintray/JCenter and to the Nexus - it has common part:
```kotlin
dependencies {
    `maven-publish`
    id("com.jfrog.bintray") version Versions.binTrayPlugin // only for Bintray publishing
}

repositories {
    jcenter()
}

// deployment configuration - deploy with sources and documentation
val sourcesJar by tasks.creating(Jar::class) {
    archiveClassifier.set("sources")
    from(sourceSets.main.get().allSource)
}

val javadocJar by tasks.creating(Jar::class) {
    archiveClassifier.set("javadoc")
    from(tasks.javadoc)
}
```

### Deployment to Bintray/JCenter
```kotlin
val publicationName = "ktoolz"
publishing {
    publications {
        register(publicationName, MavenPublication::class) {
            from(components["java"])
            artifact(sourcesJar)
            artifact(javadocJar)
        }
    }
}

bintray {
    user = "user"
    key = "password"
    publish = true
    setPublications(publicationName)
    pkg(delegateClosureOf<BintrayExtension.PackageConfig> {
        repo = "maven"
        name = "ktoolz"
        userOrg = "blindspot-ai"
        websiteUrl = "https://blindspot.ai"
        githubRepo = "blindspot-ai/ktoolz"
        vcsUrl = "https://github.com/blindspot-ai/ktoolz"
        description = "Collection of Kotlin extension functions and utilities."
        setLabels("kotlin")
        setLicenses("MIT")
        desc = description
    })
}
```

### Deploy to Nexus artifactory
```kotlin
publishing {
    publications {
        create<MavenPublication>("default") {
            from(components["java"])
            artifact(sourcesJar)
            artifact(javadocJar)
        }
    }
    repositories {
        maven {
            val releasesRepoUrl = URI(Props.nexusUrlReleases.get())
            val snapshotsRepoUrl = URI(Props.nexusUrlSnapshots.get())

            url = if (version.toString().endsWith("SNAPSHOT")) snapshotsRepoUrl else releasesRepoUrl

            credentials {
                username = Props.nexusUser.get()
                password = Props.nexusPassword.get()
            }
        }
    }
}
```