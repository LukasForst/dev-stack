# Libraries
Cool libraries I use.

## Ktoolz
Collection of extension functions 
```kotlin
repositories {
    jcenter()
}

dependencies {
    implementation("ai.blindspot.ktoolz:ktoolz:$ktoolz_version")
}
```
## Arrow
More functional approach in Kotlin - https://arrow-kt.io/
```kotlin
repositories {
    mavenCentral()
    jcenter()
    maven { url("https://dl.bintray.com/arrow-kt/arrow-kt/") } 
}

dependencies {
    implementation("io.arrow-kt:arrow-core:$arrow_version")
    implementation("io.arrow-kt:arrow-syntax:$arrow_version")
}
```
