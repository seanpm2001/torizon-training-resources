# TOR: Introduction to Torizon

## Torizon Bootcamp with Sergio Prado

<img src="/training/images/tor/torizon-marketing.png"
     alt="Torizon"
     width="80%" />

===

## About this document

* Torizon Bootcamp training slides v2.0.1.
* Latest version of the slides is available in the link below:  
[https://e-labworks.com/training/en/tor/slides](https://e-labworks.com/training/en/tor/slides)
* This document is made available under the Creative Commons Attribution-ShareAlike 4.0 International License ([CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)).
* This training (and all related material) is the result of a collaboration between Embedded Labworks and Toradex.

![Embedded Labworks](/training/images/elabworks-logo.png)

===

## About the instructor

* Sergio Prado has 25+ years of experience in software development for embedded systems, working mainly with BSP development in projects with embedded Linux, embedded Android and real-time operating systems.
* Consultant & Trainer at Embedded Labworks, providing consulting, training and software development services for embedded systems.  
[https://e-labworks.com/en](https://e-labworks.com/en)
* Free software contributor (Buildroot, Yocto Project, Linux kernel, etc).
* Blog and social media:  
[https://sergioprado.blog](https://sergioprado.blog)  
[https://www.linkedin.com/in/sprado](https://www.linkedin.com/in/sprado)  
[https://twitter.com/sergioprado](https://twitter.com/sergioprado)

===

## Training agenda

* **Topic 1**: Introduction to Torizon (Torizon OS, Torizon Cloud).
  * Exercise: First steps with Torizon.
* **Topic 2**: Image customization with TorizonCore Builder.
  * Exercise: Customizing Torizon OS images with TorizonCore Builder.
* **Topic 3**: Application development in Torizon.
  * Exercise: Developing containerized applications with Docker.
* **Topic 4**: Developing with Visual Studio Code.
  * Exercise: Application development with VS Code

===

## Not covered in this training

* BSP development (U-Boot, kernel, device drivers, etc).
* Embedded Linux concepts (toolchain, bootloader, kernel, rootfs).
* OpenEmbedded, Yocto Project and how to build Torizon OS from sources.
* Programming languages.
* Advanced Docker concepts.

===

## Hardware: Verdin iMX8MP + Yavia base board

<img src="/training/images/tor/toradex-verdin-yavia.png"
     alt="Verdin iMX8MP + Yavia base board"
     width="42%" />

===

## Prerequisites

* Basic knowledge of GNU/Linux command line tools (*cat*, *echo*, *vim*, *grep*, *find*, etc).
* Embedded Linux architecture (toolchain, bootloader, kernel, rootfs).
* Basic programming in C and C++.

===

## Labs environment

```Shell Session
$ tree /opt/labs/
/opt/labs/
├── dl
│   ├── tezi.tar.bz2
│   ├── torizon-core-docker-verdin-imx8mp-Tezi_6.2.0+build.2.tar
│   └── torizon-core-docker-verdin-imx8mp-Tezi_6.3.0+build.4.tar
├── docs
│   ├── hardware
│   │   ├── verdin-imx8m-plus-datasheet-v1.1.pdf
│   │   ├── yavia-v1.0-datasheet.pdf
│   │   └── yavia-v1.0-schematics.zip
│   ├── slides.pdf
│   └── version.txt
├── ex
└── tools
    ├── docker-test
    │   └── Dockerfile
    ├── prepare.cfg
    ├── prepare.sh
    └── tcb-env-setup.sh
```

===

## During the training...

* Ask...
* Express your point of view...
* Share your experience...
* Help...
* Participate!

===

## Torizon Bootcamp with Sergio Prado

### Introduction to Torizon

===

## What is an embedded Linux system?

<img src="/training/images/linux-arch-2.png"
     alt="Linux System Architecture"
     width="70%" />

===

## The challenges

* Initial operating system image ready for application development.
* Cross-compiling tools for development and debugging.
* Image customization during development.
* Generation of production image for device provisioning.
* Operating system maintenance over time.

===

## The options

* Use a (generic) third-party operating system:
  * Debian/Ubuntu.
  * Android.
  * Many others!
* Build and maintain a custom Linux distribution:
  * Buildroot.
  * OpenWRT.
  * OpenEmbedded (Yocto Project).

===

## 3rd party vs custom

|                          | Third-party OS | Custom OS |
| :----------------------- | :------------: | :-------: |
| Ready to be used | <span style="color:green">**Yes**</span> | <span style="color:red">**No**</span> |
| Optimized for the hardware | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |
| Easier to extend | <span style="color:green">**Yes**</span> | <span style="color:red">**No**</span> |
| Easier to create image for provisioning | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |
| Focus on application development | <span style="color:green">**Yes**</span> | <span style="color:red">**No**</span> |
| Total control over OS content | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |
| Friendly development environment | <span style="color:orange">**Maybe**</span> | <span style="color:red">**No**</span> |

===

## 3rd party vs custom (cont.)

|                          | Third-party OS | Custom OS |
| :----------------------- | :------------: | :-------: |
| Easier to learn | <span style="color:green">**Yes**</span> | <span style="color:red">**No**</span> |
| Built-in features for embedded usage | <span style="color:red">**No**</span> | <span style="color:orange">**Maybe**</span> |
| Reduced time-to-market | <span style="color:green">**Yes**</span> | <span style="color:red">**No**</span> |
| Improved boot time | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |
| Improved security | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |
| License compliance management | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |
| "Free" OS maintenance | <span style="color:red">**No**</span> | <span style="color:red">**No**</span> |

===

## What is Torizon?

* Torizon is a platform that aims to simplify embedded Linux development.
* As easier as working with a third-party operating system:
  * Provides a ready-to-be-used Linux distribution called Torizon OS.
  * Applications are deployed as container images.
  * Provides tools (e.g. VS Code extension) for application development.
* As flexible as working with a build system:
  * Helper tools (e.g. TorizonCore Builder) to customize the image.
  * Several features already available (OTA update, device monitoring, etc).
  * Can be compiled from sources if needed.

===

## Torizon

<img src="/training/images/tor/torizon-platform.png"
     alt="Torizon"
     width="40%" />

===

## 3rd party vs custom vs Torizon

|                          | Third-party OS | Custom OS | Torizon |
| :----------------------- | :------------: | :-------: | :-----: |
| Ready to be used | <span style="color:green">**Yes**</span> | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |
| Optimized for the hardware | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> | <span style="color:green">**Yes**</span> |
| Easier to extend | <span style="color:green">**Yes**</span> | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |
| Easier to create image for provisioning | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> | <span style="color:green">**Yes**</span> |
| Focus on application development | <span style="color:green">**Yes**</span> | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |
| Total control over OS content | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> | <span style="color:orange">**Maybe**</span> |
| Friendly development environment | <span style="color:orange">**Maybe**</span> | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |

===

## 3rd party vs custom vs Torizon (cont.)

|                          | Third-party OS | Custom OS | Torizon |
| :----------------------- | :------------: | :-------: | :-----: |
| Easy to learn | <span style="color:green">**Yes**</span> | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |
| Built-in features for embedded usage | <span style="color:red">**No**</span> | <span style="color:orange">**Maybe**</span> | <span style="color:green">**Yes**</span> |
| Reduced time-to-market | <span style="color:green">**Yes**</span> | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |
| Improved boot time | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> | <span style="color:green">**Yes**</span> |
| Improved security | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> | <span style="color:green">**Yes**</span> |
| License compliance management | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> | <span style="color:green">**Yes**</span> |
| "Free" OS maintenance | <span style="color:red">**No**</span> | <span style="color:red">**No**</span> | <span style="color:green">**Yes**</span> |

===

## Torizon components

* **Torizon OS**: ready-to-be-used Linux distribution.
* **TorizonCore Builder**: tool to customize Torizon OS images.
* **Visual Studio Code extension**: makes it easier to develop containerized applications.
* **Reference container images**: ready-to-be-used container images to accelerate the development of containerized applications (Weston, Qt5, Chromium, etc).
* **Torizon Cloud**: device and fleet management, OTA update system, offline updates, device monitoring, remote access, etc.

===

## Torizon OS

* Torizon OS (previously called TorizonCore) is the Linux distribution for Torizon.
  * Optimized for Toradex hardware.
  * Container-based operating system.
  * Integrated with Torizon Cloud (OTA and offline updates, device and fleet management, device monitoring, remote access, etc).
* Built with OpenEmbedded/Yocto Project and open source!  
[https://github.com/toradex/meta-toradex-torizon](https://github.com/toradex/meta-toradex-torizon)

===

## Torizon OS architecture

<img src="/training/images/tor/torizoncore-arch.png"
     alt="Torizon OS architecture"
     width="60%" />

===

## Torizon OS building blocks

* U-Boot and the Linux kernel from Toradex BSP.
* OSTree based rootfs.
* systemd and basic services (journald, dbus, usermount, etc).
  * Network services (NetworkManager, Wireguard, etc).
  * Hardware services (ModemManager, rngd, etc).
* Container engine (dockerd, containerd, runc).
* Integration with Torizon Cloud (Aktualizr-Torizon, Greenboot, Fluent-Bit, RAC).

===

## Integration with Torizon Cloud

* Aktualizr-Torizon is the client application that communicates with Torizon Cloud.  
[https://github.com/toradex/aktualizr-torizon](https://github.com/toradex/aktualizr-torizon)
* It is Uptane-compliant, an open and secure standard for software updates in the automotive industry.  
[https://uptane.github.io/](https://uptane.github.io/)
* Provides end-to-end encryption, guaranteeing integrity and confidentiality during the communication.

===

## Torizon Cloud features

* The integration with Torizon Cloud (previously Torizon Platform) makes it possible to have a convenient and secure hosted solution for connected services, including:
  * Remote and offline updates.
  * Device and fleet management.
  * Device monitoring.
  * Remote access.
* For a complete list of provided services, see the [official documentation](https://developer.toradex.com/torizon/torizon-platform/torizon-platform-services-overview).
* Available in free and paid options.  
[https://www.torizon.io/pricing](https://www.torizon.io/pricing)

===

## Torizon update system

* Aktualizr-Torizon is the program responsible for communicating with Torizon Cloud.
* For Aktualizr-Torizon to start communicating with Torizon Cloud, the device needs to be provisioned ([manually](https://developer.toradex.com/torizon/torizon-platform/devices-fleet-management#provisioning-a-single-device) or [at scale](https://developer.toradex.com/torizon/torizoncore/best-practices/production-programming-in-torizon/)).
* As soon as the device is provisioned, Aktualizr-Torizon will poll the server for new updates, download and apply them.
  * The communication is based on the Uptane standard.
  * It supports bootloader, operating system (OS) and application updates.
* For operating system updates, Torizon OS uses OSTree.

===

## OSTree

* OSTree, also known as *libostree*, provides a "git-like" model for committing and downloading bootable filesystem trees (rootfs).
* Every new version of the OS is a commit in the OSTree repository that can be checked out in an atomic way.
  * Update is faster because only the difference (new objects) are sent to the device.
  * Update is reliable due to the atomic nature of the checkout operation.
  * Rollback is possible since the previous version of the OS is kept in the repository.
* For details on how OSTree works, watch the talk [Designing OSTree based embedded Linux systems](https://www.youtube.com/watch?v=skDBX9Rywv8) (Live Embedded Event - Sergio Prado - 2021).

===

## Rootfs organization with OSTree

| Directory | Description |
| :-------: | :---------- |
| */usr* | Operating system binaries (executables, libraries, base configuration files, etc), bind-mounted read-only from an OSTree deployment, which contains hard-links to objects in the repository |
| */etc* | Operating system configuration files, handled in a "special" way. It starts with the contents of */usr/etc* from the initial deployment, but can be changed, and the changes are kept during new deployments (i.e. updates) |
| */var* | Run-time generated data, stored outside of the OSTree repository and shared between deployments (this is where container images are stored) |

===

## OSTree advantages

* There are several advantagens on using OSTree when compared to other alternatives (A/B update systems, package manager update system, etc):
  * Automatic de-duplication and space savings.
  * Minimal update size (lower network bandwidth consumption).
  * Built-in integrity checks.
  * Atomic updates.
  * Easy rollbacks.

===

## How rollback works?

* By default, when an update is in progress, Torizon OS will try to sucessfully boot the system 3 times.
  * There are several checks in the system that will force a reboot during an update, in case something fails (kernel panic, a process hangs, docker doesn't run, etc).
  * In case the system reboots 3 times without confirming the update, it rolls back to the previous version.
  * The update is confirmed if the system successfully boots, and Aktualizr-Torizon is able to run and confirm the update to Torizon Cloud.
* Greenboot is a framework integrated in Torizon OS that can be used to customize update checks and the roolback logic.  
[https://github.com/fedora-iot/greenboot](https://github.com/fedora-iot/greenboot)

===

## Secure offline updates

* Secure offline updates make it possible to update devices that have limited bandwidth or have no network connection at all.
* It is based on the same technologies used in remote updates (OSTree, Aktualizr-Torizon, etc) and follows the [Uptane PURE-2](https://github.com/uptane/pures/blob/main/pure2.md) specification.
* TorizonCore Builder is used to create and sign offline images (also called Lockbox).
  * The Lockbox can be saved on any storage medium (USB, SD Card, etc).
  * The update is automatically triggered when the storage medium is connected to the device.
* For more information, check the [official documentation](https://developer.toradex.com/torizon/torizon-platform/torizon-updates/first-steps-with-secure-offline-updates/).

===

## Device monitoring

* Torizon OS ships with a monitoring program called Fluent Bit, a fast and lightweight telemetry agent for collecting metrics, logs and system alerts.  
[https://fluentbit.io/](https://fluentbit.io/)
* It is easily configurable and process data in a pipeline via plugins (input, parser, filter, output).
* The data is sent via a secure communication channel provided by Aktualizr-Torizon.
* It is enabled by default when a device is provisioned, and can be easily customized via configuration files in */etc/fluent-bit*.
* For more information, check the [official documentation](https://developer.toradex.com/torizon/torizon-platform/device-monitoring-in-torizoncore/).

===

## Remote access

* Remote access is a feature that allows to access the terminal of a Torizon OS target device via SSH.
  * Makes it possible to test, launch and manage the system without a physical connection, minimizing the costs with on-site visits.
  * Can be useful for troubleshooting (e.g. debugging in production) and maintenance (e.g. changing device configuration).
* On the device, a [Remote Access Client (RAC)](https://github.com/toradex/torizon-rac) program is used for:
  * Securely exchange keys with Torizon Cloud for the remote connection.
  * Spawn a *sshd* instance to accept the remote connection.

===

## Torizon and DevOps

* Manual maintenance of large device fleets is impractical and sometimes impossible!
  * Being faster to identify and fix bugs in the field is essential in the modern world of connected devices.
* To help with that, Torizon is built with a DevOps mindset!
  * DevOps is the combination of practices and tools designed to increase the ability to deliver applications faster than traditional software development processes.
* Torizon Cloud provides a REST API that can be easily integrated into a CI/CD workflow.  
[https://developer.toradex.com/torizon/torizon-platform/torizon-api/](https://developer.toradex.com/torizon/torizon-platform/torizon-api/)

===

## Exercise 1

### First steps with Torizon
