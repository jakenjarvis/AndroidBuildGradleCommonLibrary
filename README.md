AndroidBuildGradleCommonLibrary
===============================

This is a my gradle common library for Android project. Add "git submodule" to the project. Used by the "apply from" from the "build.gradle".


Setup
------------------------------

    git submodule add git@github.com:jakenjarvis/AndroidBuildGradleCommonLibrary.git gradle/library
    git submodule update --init
    git commit -m "Add AndroidBuildGradleCommonLibrary as a submodule"


Update
------------------------------

    git submodule foreach git pull origin master
    git commit -m "Update AndroidBuildGradleCommonLibrary as a submodule"

