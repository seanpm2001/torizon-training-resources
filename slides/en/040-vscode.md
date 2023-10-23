# TOR: Developing with Visual Studio Code

## Torizon Bootcamp with Sergio Prado

### Developing with Visual Studio Code

===

## Visual Studio Code

* Visual Studio Code (VS Code) is a lightweight but powerful source code editor which runs on the desktop and is available for Windows, macOS and Linux.  
[https://code.visualstudio.com/](https://code.visualstudio.com/)
* It's currently one of the most popular application development environments for non-embedded applications (though the popularity is increasing on embedded).
* Extensible and customizable via extensions, enabling developers to easily add support to additional languages, debuggers, and tools.
* It comes with built-in support for JavaScript, TypeScript and Node.js, and has a rich ecosystem of extensions for other languages (C, C++, C#, Java, Python, PHP, Go, Rust, etc).

===

## Torizon extensions

* Toradex offers two VS Code extensions for Torizon.
* **Torizon IDE Extension (ApolloX)**: help users to develop, build, deploy and debug containerized applications in Torizon OS.  
[https://marketplace.visualstudio.com/items?itemName=Toradex.apollox-vscode](https://marketplace.visualstudio.com/items?itemName=Toradex.apollox-vscode)
* **TorizonCore Builder extension**: help users to create customized Torizon OS images (via the build configuration file) and push them to Torizon Cloud.  
[https://marketplace.visualstudio.com/items?itemName=Toradex.tcb-vscode](https://marketplace.visualstudio.com/items?itemName=Toradex.tcb-vscode)
* In this session, we will focus on the Torizon IDE Extension (ApolloX).

===

## Torizon IDE Extension (ApolloX)

* The Torizon IDE Extension abstracts and automates the entire development workflow:
  * Leveraging the editor capabilities for code editing.
  * Cross-compiling the application for the target platform.
  * Deploying the application inside a container (it maintains a *Dockerfile* for you!).
  * Deploying and running the containerized application on the target.
  * Starting a remote debugging session.
  * Pushing the container image to a registry (e.g. Docker Hub).
  * Pushing application updates to Torizon Cloud.

===

## Torizon extensions V1 and V2

* There are two versions of the Torizon extension: V1 and V2.
* Torizon Extension V1 is the first version of the extension, and it's available for Visual Studio (Windows-only and closed-source) and VS Code.
  * It supports only Torizon OS 5 and it's currently in maintenance mode.
* Torizon Extension V2 is the new extension (codenamed ApolloX).
  * It's the recommended development environment for Torizon OS 6+.
  * First stable version (2.0.0) released in April/2023.
  * It will be our focus in the training.

===

## Torizon Extension V2 (ApolloX)

* The ApolloX project is a complete redesign (from scratch) of the VS Code extension for containerized application development in Torizon.  
[https://labs.toradex.com/projects/torizon-vs-code-v2-apollo-x](https://labs.toradex.com/projects/torizon-vs-code-v2-apollo-x)
* Implemented via libraries written in TypeScript using a more scripted approach to tasks compared to the previous version.
  * V1 had a more complex client-server architecture, making it harder to add new features and extend its functionalities.
  * V2 has a few extra features compared to V1, including the support for multi-container projects and integration with GitHub/GitLab CI environments.
* Documentation on installing and using the extension:  
[https://developer.toradex.com/torizon/application-development/ide-extension/](https://developer.toradex.com/torizon/application-development/ide-extension/)

===

## Torizon Extension Templates

* Templates are example projects that can be used as a base when creating new projects with the Torizon Extension.
* Templates officially supported by Toradex: C/C++ (Makefile, Qt6 QML), Python 3 (console, Qt 5 Pyside 2 QML), C#/.NET (console, Uno Platform), etc.
* Templates supported by partners: Slint (C++/Rust).
* Templates supported by the community: Rust (console), Java (Swing JFrame), Node.js (console, Electron Javascript), .NET/C# (Mono 4.7 Windows Forms, Uno Platform, Avalonia), Visual Basic (Gambas3 Forms), etc.
* Templates for supported use cases are maintained in the following repository:  
[https://github.com/toradex/vscode-torizon-templates](https://github.com/toradex/vscode-torizon-templates)

===

## Installing the extension

* The first step is to download and install Visual Studio Code:  
[https://code.visualstudio.com/download](https://code.visualstudio.com/download)
* Currently, only Ubuntu, Debian and Windows Subsystem for Linux (WSL 2) are officially supported.
* Some dependencies like Docker, Docker Compose and PowerShell need to be installed on the host machine.
* The complete installation process is documented on [Toradex's developer website](https://developer.toradex.com/torizon/application-development/ide-extension/set-up-the-ide-ext2-environment/).

===

## Using the extension

* The first step is to connect to a Torizon OS target device (the extension is able to scan the network and automatically connect to a device running Torizon OS).  
[Connect a Torizon OS Target Device](https://developer.toradex.com/torizon/application-development/ide-extension/connect-a-torizoncore-target-device/)
* Next step is to create a new project from a template provided by the extension:  
[Create a Single-Container Project](https://developer.toradex.com/torizon/application-development/ide-extension/create-single-container-projects/)
* Then you can develop, compile, deploy and debug directly on the target.  
[Remote Deploy and Debug Projects](https://developer.toradex.com/torizon/application-development/ide-extension/remote-debug-and-deploy-projects/)
* When development is finished, you can push the image to production!  
[Build and Push Applications](https://developer.toradex.com/torizon/application-development/ide-extension/generating-production-images/)

===

## Installing

<img src="/training/images/tor/vscode-installing.gif"
     alt="Installing Visla Studio Code"
     width="60%" />

===

## Activating

<img src="/training/images/tor/vscode-activating.gif"
     alt="Activating the Torizon Extension"
     width="60%" />

===

## Scanning devices

<img src="/training/images/tor/vscode-devices-scanning.gif"
     alt="Scanning devices"
     width="60%" />

===

## Connecting to a device

<img src="/training/images/tor/vscode-devices-connecting.gif"
     alt="Connecting to a device"
     width="60%" />

===

## Setting device as default

<img src="/training/images/tor/vscode-devices-setting-default.gif"
     alt="Setting device as default"
     width="60%" />

===

## Creating a new Torizon project

<img src="/training/images/tor/vscode-project-creating.gif"
     alt="Creating a new Torizon project"
     width="60%" />

===

## Choosing a template

<img src="/training/images/tor/vscode-project-template-choosing.gif"
     alt="Choosing a template"
     width="60%" />

===

## Configuring the template

<img src="/training/images/tor/vscode-project-template-configuring.gif"
     alt="Configuring the template"
     width="60%" />

===

## Opening the new project

<img src="/training/images/tor/vscode-project-source-code.gif"
     alt="Project source code"
     width="60%" />

===

## Remote debugging

<img src="/training/images/tor/vscode-project-remote-debugging.gif"
     alt="Remote debugging"
     width="60%" />

===

## Pushing to a registry

<img src="/training/images/tor/vscode-project-registry-pushing.gif"
     alt="Pushing to a registry"
     width="60%" />

===

## Exercise 4

### Application development with VS Code
