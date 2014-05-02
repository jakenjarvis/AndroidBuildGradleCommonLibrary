# AndroidBuildGradleCommonLibrary

This is a my gradle common library for Android project. Add "git submodule" to the project. Used by the "apply from" from the "build.gradle".


## Setup

    git submodule add git@github.com:jakenjarvis/AndroidBuildGradleCommonLibrary.git gradle/commonlibrary
    git submodule update --init
    git commit -m "Add AndroidBuildGradleCommonLibrary as a submodule"


## Update

    git submodule foreach git pull origin master
    git commit -m "Update AndroidBuildGradleCommonLibrary as a submodule"


## Usage example

### build.gradle (root project) 

    // CommonLibraryPlugin will be added.
    apply from: "gradle/commonlibrary/CommonLibrary.gradle"

    // Use the "commonlibrary" extension.
    commonlibrary {
        // Specify the library module you want to injection in the "apply" method.
        // This will search for partial matches the library module file name.
        apply project, "allprojectsSetEncoding"
        apply project, "allprojectsSetCompatibilityJavaVersion"

        // patch
        apply project, "allprojectsPatchDisableLintOptionsAbortOnError"
        apply project, "allprojectsPatchEnableAaptOptionsUseAaptPngCruncher"
    }

----------

This "commonlibrary" extension is a wrapper thin "apply from". Path is automatic search from /gradle.

    commonlibrary {
        apply project, "allprojectsSetEncoding"
    }

This is equivalent to the following syntax.

    apply from: "gradle/commonlibrary/allprojectsSetEncoding.gradle"


sample: build.gradle (root project)  
https://github.com/jakenjarvis/Android-SequentialTask/blob/master/build.gradle

----------

### build.gradle (android library sub project) 

    commonlibrary {
        // This is the library module to which you want to add the task.
        apply project, "addTaskArtifactAar"
        apply project, "addTaskArtifactApklib"
        apply project, "addTaskArtifactJar"
        apply project, "addTaskArtifactJavadocJar"
        apply project, "addTaskArtifactSourceJar"
    }

    artifacts {
        afterEvaluate {
            archives packageArtifactReleaseAar
            archives packageArtifactReleaseApklib
            archives packageArtifactReleaseJar
            archives packageArtifactReleaseSourceJar
            archives packageArtifactReleaseJavadocJar
        }
    }

    android.libraryVariants
    publishing {
        publications {
            releaseArtifact(MavenPublication) {
                // Note: Because the task is automatically added by the AndroidGradlePlugin,
                // will not be able to recognize the task If it is not after evaluation.
                afterEvaluate {
                    artifact packageArtifactReleaseAar
                    artifact packageArtifactReleaseApklib
                    artifact packageArtifactReleaseJar
                    artifact packageArtifactReleaseSourceJar
                    artifact packageArtifactReleaseJavadocJar
                }
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


### build.gradle (android application sub project) 

    commonlibrary {
        // This is the library module to which you want to add the task.
        // Task is added to "artifacts.archives" automatically.
        apply project, "addTaskArtifactApk"
        apply project, "addTaskArtifactJavadocJar"
        apply project, "addTaskArtifactSourceJar"
    }

    artifacts {
        afterEvaluate {
            archives packageArtifactReleaseApk
            archives packageArtifactReleaseSourceJar
            archives packageArtifactReleaseJavadocJar
        }
    }

    android.applicationVariants
    publishing {
        publications {
            releaseArtifact(MavenPublication) {
                // Note: Because the task is automatically added by the AndroidGradlePlugin,
                // will not be able to recognize the task If it is not after evaluation.
                afterEvaluate {
                    artifact packageArtifactReleaseApk
                    artifact packageArtifactReleaseSourceJar
                    artifact packageArtifactReleaseJavadocJar
                }
            }
        }
        repositories {
            maven {
                url(mavenRepository)
            }
        }
    }

