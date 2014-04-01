# AndroidBuildGradleCommonLibrary

This is a my gradle common library for Android project. Add "git submodule" to the project. Used by the "apply from" from the "build.gradle".


## Setup

    git submodule add git@github.com:jakenjarvis/AndroidBuildGradleCommonLibrary.git gradle/library
    git submodule update --init
    git commit -m "Add AndroidBuildGradleCommonLibrary as a submodule"


## Update

    git submodule foreach git pull origin master
    git commit -m "Update AndroidBuildGradleCommonLibrary as a submodule"


## Usage example

### build.gradle (root project) 

    // CommonLibraryPlugin will be added.
    apply from: "gradle/library/CommonLibrary.gradle"

    // Use the "commonlibrary" extension.
    commonlibrary {
        // Specify the library module you want to injection in the "apply" method.
        // This will search for partial matches the library module file name.
        apply project, "allprojectsSetEncoding"
        apply project, "allprojectsSetCompatibilityJavaVersion"

        // patch
        apply(project, "allprojectsPatchDisableLintOptionsAbortOnError")
        apply(project, "allprojectsPatchEnableAaptOptionsUseAaptPngCruncher")
    }

----------

This "commonlibrary" extension is a wrapper thin "apply from".

    commonlibrary {
        apply project, "allprojectsSetEncoding"
    }

This is equivalent to the following syntax.

    apply from: "gradle/library/allprojectsSetEncoding.gradle"


sample: build.gradle (root project)  
https://github.com/jakenjarvis/Android-SequentialTask/blob/master/build.gradle

----------

### build.gradle (android sub project) 

    commonlibrary {
        // This is the library module to which you want to add the task.
        apply project, "addTaskArtifactAar"
        apply project, "addTaskArtifactApklib"
        apply project, "addTaskArtifactJar"
        apply project, "addTaskArtifactJavadocJar"
        apply project, "addTaskArtifactSourceJar"
    }

    artifacts {
        archives artifactAar
        archives artifactApklib
        archives artifactJar
        archives artifactSourceJar
        archives artifactJavadocJar
    }

    android.libraryVariants
    publishing {
        publications {
            releaseAar(MavenPublication) {
                artifact artifactAar
            }
            releaseApklib(MavenPublication) {
                artifact artifactApklib
            }
            releaseJar(MavenPublication) {
                artifact artifactJar
            }
            releaseSourceJar(MavenPublication) {
                artifact artifactSourceJar
            }
            releaseJavaDocJar(MavenPublication) {
                artifact artifactJavadocJar
            }
        }
        repositories {
            maven {
                url(mavenRepository)
            }
        }
    }

sample: build.gradle (android library sub project)  
https://github.com/jakenjarvis/Android-SequentialTask/blob/master/SequentialTask/build.gradle
