CodeAlpha_GradleJavaApp

A Java application built and managed with Gradle, with automated tests and a CI/CD pipeline using GitHub Actions.

Built as part of the CodeAlpha DevOps Internship (Task 3: Java Application using Gradle).

Table of Contents
Overview
Tech Stack
Project Structure
Prerequisites
Getting Started
Gradle Commands
Dependency Management
Testing
CI/CD Pipeline
DevOps Concepts Demonstrated

# Overview

This project demonstrates how Gradle automates the build lifecycle of a Java application:

Compiling source code
Managing external dependencies (Guava, SLF4J)
Running unit tests with JUnit 5
Packaging the app into a JAR
Running a CI/CD pipeline on every push

The application contains a simple add method (exposed through App.java) with unit tests, and logs output using SLF4J.

# Tech Stack

Tool	Purpose
Java 17	Programming language
Gradle (with Wrapper)	Build automation and dependency management
JUnit 5	Unit testing
Guava	Utility library (dependency example)
SLF4J	Logging
GitHub Actions	CI/CD pipeline

# Project Structure

CodeAlpha_GradleJavaApp/
├── .github/
│   └── workflows/
│       └── ci.yml            # CI/CD pipeline
├── app/
│   ├── build.gradle          # Build config and dependencies
│   └── src/
│       ├── main/java/org/example/App.java
│       └── test/java/org/example/AppTest.java
├── gradle/wrapper/           # Gradle Wrapper files
├── gradlew                   # Wrapper script (Linux/Mac)
├── gradlew.bat               # Wrapper script (Windows)
├── settings.gradle
├── .gitignore
└── README.md

# Prerequisites
JDK 17 or higher (java -version)
Git

Gradle does not need to be installed separately. The included wrapper (gradlew) downloads the correct version automatically.

# Getting Started

1. Clone the repository
git clone https://github.com/<your-username>/CodeAlpha_GradleJavaApp.git
cd CodeAlpha_GradleJavaApp

2. Make the wrapper executable (Linux/Mac/WSL)
chmod +x gradlew

3. Build the project
./gradlew clean build

4. Run the application
./gradlew run

Expected output:

Sum: 30

# Gradle Commands
Command	Description
./gradlew clean	Delete the build/ directory
./gradlew build	Compile, test and package the app
./gradlew test	Run unit tests only
./gradlew run	Run the application
./gradlew :app:dependencies	Show the dependency tree
./gradlew tasks	List all available tasks

The built JAR is located at app/build/libs/.

# Dependency Management

Dependencies are declared in app/build.gradle and downloaded automatically from Maven Central:

groovy
dependencies {
    implementation 'com.google.guava:guava:33.0.0-jre'
    implementation 'org.slf4j:slf4j-simple:2.0.13'
    testImplementation 'org.junit.jupiter:junit-jupiter:5.10.2'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}
implementation: needed to compile and run the app
testImplementation: needed only for tests

View the resolved dependency tree:

bash
./gradlew :app:dependencies --configuration runtimeClasspath

# Testing

Unit tests are in app/src/test/java/org/example/AppTest.java and cover positive numbers, negative numbers and zero.

bash
./gradlew test

The HTML test report is generated at:

app/build/reports/tests/test/index.html

# CI/CD Pipeline

The pipeline is defined in .github/workflows/ci.yml and runs automatically on every push and pull request:

Check out the code
Set up JDK 17 (Temurin)
Set up Gradle with caching
Run ./gradlew build (compile + test)
Upload the built JAR as a downloadable artifact
yaml
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with: { distribution: temurin, java-version: 17 }
      - uses: gradle/actions/setup-gradle@v3
      - run: ./gradlew build
      - uses: actions/upload-artifact@v4
        with:
          name: app-jar
          path: app/build/libs/*.jar

Pipeline status: see the Actions tab of this repository.

# DevOps Concepts Demonstrated
Build automation: one command compiles, tests and packages the app
Dependency management: declarative dependencies resolved from Maven Central
Reproducible builds: the Gradle Wrapper pins the Gradle version
Continuous Integration: every push is built and tested automatically
Continuous Delivery: the JAR is published as a build artifact
Version control: .gitignore excludes build/ and .gradle/
