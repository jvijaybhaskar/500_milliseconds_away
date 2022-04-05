# SDKMAN!

SDKMAN! is a tool for managing parallel versions of multiple Software Development Kits on most Unix-based systems. It provides a convenient Command Line Interface (CLI) and API for installing, switching, removing, and listing Candidates.

SDKMAN! is not just a JDK manager; it supports many more SDKs such as Maven, Gradle, Springboot, and Micronaut! This tool helps us manage multiple JDK installations (and installations of other SDKs) and also allows developers to configure each codebase to use a specific JDK version without the hassle of changing the JAVA_HOME environment variable.

The only thing you need is a terminal to make use of SDKMAN!

---


## Install SDKMAN!
For the latest installation instructions, visit the [official installation guide](https://sdkman.io/install).



## Cheatsheet
To check if SDKMAN! has been successfully installed:
`sdk version`

Install the latest stable version of JDK:
`sdk install java`

Install specific version of JDK:
`sdk install java <>`

Setting default global JDK version:
`sdk default java <lts>`

Remove an installed version:
`sdk uninstall java <>`

To see all available candidates:
`sdk list`

To see all available candidate versions:
`sdk list java`

To use a candidate version:
`sdk use java <>`
Note: This will switch the version for the current shell only.

What is currently in use for a candidate:
`sdk current java`

The current version of all candidates:
`sdk current`

Switch to a specific JDK or SDK every time you visit a project

```sh
sdk env init
#.sdkmanrc file is created in the current directory

nano .sdkmanrc 
# add java=<version that your prefer> and save

# Issue the following command to switch to configuration present in .sdkmanrc
set env

```

If you want to reset SDKs to default version when leaving the project:
`sdk env clear`

You have just checked out a new project from the repo and want to install missing SDK, then run:
`sdk env install`

---


## References
https://sdkman.io/usage#default 
https://sdkman.io/
https://sdkman.io/install