# android-studio-gradle-test
A test project with a structure to stress test and find out issues in Android Studio and Gradle

Also provides an alternative build path using [Buck](https://buckbuild.com/) to compare. For more details on how the buck build path is setup, please see [OkBuck](https://github.com/uber/okbuck)

[![Master branch build status](https://travis-ci.org/kageiit/android-studio-gradle-test.svg?branch=master)](https://travis-ci.org/kageiit/android-studio-gradle-test)

## Prerequisites

This project uses legacy build tools (Gradle 3.4 and Android Gradle Plugin 2.3.0) and requires a specific environment:

- **Java JDK 8**: Mandatory. Higher versions will fail to build.
- **Android SDK**: Ensure `ANDROID_HOME` is set or a `local.properties` file exists with `sdk.dir`.

### Installing Java 8

If you don't have Java 8 installed, we recommend using [SDKMAN!](https://sdkman.io/) or Homebrew:

**Using SDKMAN! (Cross-platform):**
```bash
sdk install java 8.0.402-tem
sdk use java 8.0.402-tem
```

**Using Homebrew (macOS):**
```bash
brew tap homebrew/cask-versions
brew install --cask zulu8
```

## Setup

1.  **Configure Java 8**: Ensure your terminal is using Java 8.
    ```bash
    # For macOS (Homebrew/Standard)
    export JAVA_HOME=$(/usr/libexec/java_home -v 1.8)
    
    # For Linux
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 # Path may vary
    
    export PATH=$JAVA_HOME/bin:$PATH
    ```
2.  **Generate Sources**: This project generates its source code dynamically. You **must** run this before building.
    ```bash
    ./gradlew addSources
    ```

## To build all apps with gradle:
```bash
./gradlew assembleDebug
```

## Troubleshooting

### SAXParseException: '37.0' is not a valid value for 'integer'
If you encounter this error, it is due to a bug in legacy Android Gradle Plugin versions when parsing newer Android SDK component metadata. The build script has been configured to use online repositories (`maven.google.com`) to avoid local parsing issues. Ensure you have an active internet connection for the first build.

### Java Version Mismatch
If the build fails with "This build must be run with Java 8", check your `java -version` and ensure `JAVA_HOME` points to a JDK 1.8 installation.


## To build all apps with buck

### Setup
#### Mac OS X
```bash
brew update
brew install ant watchman
```

#### Linux
Installation instructions for: [Ant](http://ant.apache.org/), [Watchman](https://facebook.github.io/watchman/docs/install.html)

### Build
```bash
./buildWithBuck
```

## Benchmarking and profiling (Experimental)

Run `./gradlew addSources` to generate source code for all subprojects.

Use the Gradle profiler to `--benchmark` or `--profile` scenarios. The available scenarios are defined in `performance.scenarios`

Example usage: `./gradle-profiler --profile chrome-trace upToDateSingleVariant`
