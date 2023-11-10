# Exercises - Torizon Bootcamp

Torizon Bootcamp exercises.

|                   |                     |
| :---------------- | :------------------ |
| **Target**        | Verdin iMX8MP       |
| **Language**      | English (US)        |
| **Version**       | 2.0.1               |

The most recent version of this document can be found on Embedded Labworks's website:

[https://e-labworks.com/training/en/tor/labs](https://e-labworks.com/training/en/tor/labs)

This document is made available under the Creative Commons Attribution-ShareAlike 4.0 International License (CC BY-SA 4.0).

[https://creativecommons.org/licenses/by-sa/4.0/](https://creativecommons.org/licenses/by-sa/4.0/)

## Index <!-- omit in toc -->

- [About this document](#about-this-document)
  - [Guidelines](#guidelines)
  - [Conventions](#conventions)
  - [Substitution commands](#substitution-commands)
  - [Keyboard layout](#keyboard-layout)
  - [Text editor](#text-editor)
  - [Source code](#source-code)
  - [Virtual machine](#virtual-machine)
  - [Answers](#answers)
- [Lab 1 - First steps with Torizon](#lab-1---first-steps-with-torizon)
  - [1.1 Testing the target device](#11-testing-the-target-device)
  - [1.2 Inspecting Torizon OS](#12-inspecting-torizon-os)
  - [1.3 Configuring Wi-Fi](#13-configuring-wi-fi)
  - [1.4 Remote updates](#14-remote-updates)
  - [1.5 Device monitoring](#15-device-monitoring)
  - [1.6 Remote access](#16-remote-access)
- [Lab 2 - Customizing images with TorizonCore Builder](#lab-2---customizing-images-with-torizoncore-builder)
  - [2.1 Installing TorizonCore Builder](#21-installing-torizoncore-builder)
  - [2.2 Creating a custom Torizon OS image](#22-creating-a-custom-torizon-os-image)
  - [2.3 Pushing the custom image to Torizon Cloud](#23-pushing-the-custom-image-to-torizon-cloud)
  - [2.4 Updating the device with the customized image](#24-updating-the-device-with-the-customized-image)
  - [2.5 Testing the customized image](#25-testing-the-customized-image)
- [Lab 3 - Developing containerized applications](#lab-3---developing-containerized-applications)
  - [3.1 Creating a user on Docker Hub](#31-creating-a-user-on-docker-hub)
  - [3.2 Creating a containerized web application](#32-creating-a-containerized-web-application)
  - [3.3 Testing the container in the device](#33-testing-the-container-in-the-device)
  - [3.4 Creating and triggering an application update](#34-creating-and-triggering-an-application-update)
- [Lab 4 - Application development with VS Code](#lab-4---application-development-with-vs-code)
  - [4.1 Starting Visual Studio Code](#41-starting-visual-studio-code)
  - [4.2 Connecting to the device](#42-connecting-to-the-device)
  - [4.3 Creating a new C/C++ application](#43-creating-a-new-cc-application)
  - [4.4 Writing the *blinkled* application](#44-writing-the-blinkled-application)
  - [4.5 Pushing the application to a registry](#45-pushing-the-application-to-a-registry)
  - [4.6 Creating a Compose file for remote updates](#46-creating-a-compose-file-for-remote-updates)
- [Appendix A: Flashing Torizon OS with Easy Installer](#appendix-a-flashing-torizon-os-with-easy-installer)
  - [Configuring the device in recovery mode](#configuring-the-device-in-recovery-mode)
  - [Starting the recovery program](#starting-the-recovery-program)
  - [Connecting to Easy Installer via VNC](#connecting-to-easy-installer-via-vnc)
  - [Flashing TorizonCore 6.2.0](#flashing-torizoncore-620)

## About this document

This document contains a series of activities and exercises to complement the content presented in the "Torizon Bootcamp with Sergio Prado" training.

### Guidelines

Do not hesitate to ask questions, share your experience and suggest different solutions to the problems presented in the exercises. Your participation can make the content presented richer and the training more interactive.

If you finish your lab assignments before your colleagues, help them investigate any issues they may be experiencing. The faster we progress as a group, the more chances we have to study additional topics and learn more.

Do not hesitate to report problems or errors to the instructor.

### Conventions

During lab activities, we will call *host* the development machine and *target* the development board.

Commands that must be executed in the host start with *$*:

```default
$ ls
```

Commands that must be executed in the target start with *#*:

```default
# ls
```

### Substitution commands

Expressions between angle brackets (e.g. *\<expression\>*) must be replaced according to the instructions documented in the exercises (including the signs themselves). Example:

```default
$ sudo dd if=arquivo.img of=/dev/<device> bs=1M
```

### Keyboard layout

By default, the virtual machine image is configured with the English (US) keyboard layout. You can confirm this by running the following command:

```default
$ setxkbmap -query
```

In case you want to change it, you can use the *setxkbmap* command. For example, to change the keyboard layout to German (DE):

```default
$ echo "setxkbmap -layout de" >> ~/.bashrc; source ~/.bashrc
```

### Text editor

To edit files inside the virtual machine, use the *edit* command:

```default
$ edit file.txt
```

### Source code

To facilitate the visualization when changing source code or text files, lines that must be removed will be represented by the *-* sign, and lines that must be added will be represented by the *+* sign. Don't forget to remove these symbols when copying and pasting the text. Example:

```diff
  int main(int argc, const char *argv[])
 {
-    printf("Test application V1.0\n");
+    printf("Test application V1.1\n");

     return 0;
 }
```

### Virtual machine

During the exercises, you might need to type a user and a password to run privileged commands inside the virtual machine. The default user inside the virtual machine is *torizon* and the password is also *torizon*.

### Answers

The answers to all exercises are available in the link below:

[https://e-labworks.com/training/en/tor/answers](https://e-labworks.com/training/en/tor/answers)

## Lab 1 - First steps with Torizon

In this exercise, we will learn the basics of Torizon, interact with Torizon OS and Torizon Cloud to experiment with remote updates, device monitoring and remote access.

For this exercise, it is mandatory for the module to be running TorizonCore 6.2.0. If that is not the case, before starting this exercise, please follow the instructions in [Appendix A: Flashing Torizon OS with Easy Installer](#appendix-a-flashing-torizon-os-with-easy-installer).

### 1.1 Testing the target device

Connect the host to the target via the USB cable provided by the instructor.

In VirtualBox, go to "*Devices*" -> "*USB*" and make sure "Silicon Labs CP2105 Dual USB to UART Bridget Controller" is selected.

Open a terminal and run *minicom* to start the communication with the target's serial console:

```default
$ minicom
```

Power on the target to boot Torizon OS. You should see the logs in the serial console and get a login prompt after a few seconds.

Obs.: If your host OS is Windows and you cannot get the serial port to work inside the virtual machine, try using PuTTY in Windows (you will have to go to the Device Manager to find out the correct COM port and use 115200 as the baud).

After booting Torizon OS, you can log in with user *torizon* and password *torizon*. In case you are logging in for the first time, you will be asked to change the password.

### 1.2 Inspecting Torizon OS

In the target, confirm that you are running TorizonCore 6.2.0:

```default
# cat /etc/os-release
```

Torizon OS is a typical embedded Linux system with a few popular software components (*systemd*, *dbus* and other daemons), a container runtime engine (*docker*, *containerd*) and an update client (*aktualizr-torizon*).

Check the processes running with the following command:

```default
# ps aux
```

A list of enabled services can be displayed with the command below:

```default
# systemctl list-units --type=service
```

### 1.3 Configuring Wi-Fi

Now let's configure a network connection in the target using the built-in Wi-Fi hardware available on the SoM (for the Wi-Fi hardware to work properly, make sure the antennas are connected).

First, run the following command to turn on the Wi-Fi hardware:

```default
# nmcli radio wifi on
```

Update the list of available Wi-Fi connections:

```default
# nmcli device wifi rescan
# nmcli device wifi list
```

Connect to the Wi-Fi network, replacing *\<SSID\>* and *\<PASS\>* with the connection parameters provided by the instructor:

```default
# nmcli dev wifi connect <SSID> password <PASS>
```

Confirm that you successfully connected to the Wi-Fi network:

```default
# iw dev mlan0 link
```

Confirm that the network is accessible:

```default
# ping google.com
```

### 1.4 Remote updates

Go to [https://app.torizon.io](https://app.torizon.io) and log in. In case you don't have a user, create one.

In case you didn't activate the Torizon commercial trial yet, access the dashboard, go to "*My Account*" and click on "*Activate my Commercial Tier Free Trial*". This is necessary to have access to commercial features like pushing custom OS updates and remote access.

Go to "*Devices*" and select "Provision Device".

Copy the generated command and execute it in the target. This will provision the device to Torizon Cloud.

After a few seconds, you should see the device in the dashboard. Press F5 to update the dashboard if needed.

To make the update process faster, run the following commands in the target to change the polling frequency of the update client to 5 seconds:

```default
# sudo mkdir -p /etc/sota/conf.d/
# sudo sh -c 'echo [uptane] > /etc/sota/conf.d/60-polling-interval.toml'
# sudo sh -c 'echo "polling_sec = 5" >> /etc/sota/conf.d/60-polling-interval.toml'
# sudo systemctl restart aktualizr-torizon
```

Now run the following command to monitor the update process. You should see messages from the update client (Aktualizr-Torizon) polling the server every 5 seconds.

```default
# sudo journalctl -f -u aktualizr*
```

Leave this command running while you follow the next steps.

In the Torizon Cloud's dashboard, select the device, click on "*Initiate Update*" and execute the following steps to update the device:

- Select "*verdin-imx8mp*" and click on "*Continue*".
- Select "*Torizon Release*" and disable the other options.
- In the "*Select package*" field, choose "*kirkstone/verdin-imx8mp/torizon/torizon-core-docker/release*"
- In the "*Select version*" field, choose "*6.3.0+build.4*".
- Click on "*Continue*" and "*Finish*" to initiate the remote update process.

Go back to the target's serial console and monitor the logs from the update client. Because the device is polling for updates every 5 seconds, it should start the update right away.

After receiving and applying the update, the device will automatically reboot.

If you see some messages related to *mipi_dsi* in the terminal, please ignore them. These error messages are related to the fact that the default device tree for Verdin iMX8MP is incompatible with the Yavia base board.

Log in and check the OS version to confirm that the update was successful:

```default
# cat /etc/os-release
```

In the Torizon Cloud's dashboard, go to "Devices" and confirm that the update was successful. You should see "Up to date" in the *Status* field.

### 1.5 Device monitoring

When a device is provisioned, device monitoring is enabled by default, and information about CPU usage, memory usage, CPU temperature and the Docker Service status is sent to Torizon Cloud every 5 minutes.

To visualize this information, go to the Torizon Cloud's dashboard, select the provisioned device and click on "View Detail". Since the data is sent only every 5 minutes, it might take a while for the charts to show the monitoring data.

On the device, Fluent-Bit is the service responsible for collecting and sending the monitoring data to Torizon Cloud, and can be configured via a configuration file at */etc/fluent-bit/fluent-bit.conf*.

Run the following commands in the target to change to 10 seconds the frequency the monitoring data is sent:

```default
# sudo sed -i 's/interval_sec 300/interval_sec 10/g' /etc/fluent-bit/fluent-bit.conf
# sudo systemctl restart fluent-bit
```

Now run the following command in the target to force CPU usage:

```default
# while true; do true; done &
```

Go to the Torizon Cloud's dashboard and check the device monitoring information. By default, the graphs are updated every minute, but that can be changed in the "*CHART OPTIONS*" button. You can also force an update by clicking on "*REFRESH*". After a minute or so, you should see a spike in CPU usage.

Run the command below to kill the process that is forcing CPU usage:

```default
# killall sh
```

Fluent-Bit is very flexible on how it can be configured to collect information about the device via [input plugins](https://docs.fluentbit.io/manual/pipeline/inputs). And the Torizon Cloud's dashboard is also very flexible on how it can be configured to display monitoring data via the "CUSTOMIZE METRICS" feature. Feel free to explore it.

If you have some time, try to create a custom metric to monitor disk usage by following [this article](https://developer.toradex.com/torizon/torizon-platform/device-monitoring-in-torizoncore/#example-disk-usage-custom-metric).

### 1.6 Remote access

Remote access is pre-built into Torizon OS images from 6.3 onwards and configured as a systemd service. To enable and start the service, run the following commands in the target:

```default
# sudo systemctl enable remote-access
# sudo systemctl start remote-access
```

On your host machine, run the following command to generate an ssh-rsa public key that will be used for the remote connection:

```default
$ ssh-keygen -t rsa
```

Copy the generated key to the clipboard:

```default
$ xclip -selection c ~/.ssh/id_rsa.pub
```

In the Torizon Cloud's dashboard, click on "*MY ACCOUNT*" and execute the following steps:

- Access the "*Remote Access Manager*" tab.
- Press Ctrl-V to paste the ssh-rsa public key.
- Type a name in the "*Friendly name:*" field.
- Click on "*ADD KEY*".

Now go to the "*Devices*" page, select the provisioned device and execute the following steps:

- Click on "*Remote Shell*".
- Change the duration of the session (optional).
- Click on "*INITIATE SESSION*".
- Copy the SSH command printed on the screen.
- Open a terminal on your host machine and execute the command to remotely connect to the device. You should be able to login and access the device over an SSH connection.

After you have finished the remote session, go back to the "*Remote Access Manager*" tab on the Account page and close the session at the bottom of the page, on the "*Active Remote Sessions*".

## Lab 2 - Customizing images with TorizonCore Builder

In this exercise, we will learn how to create a custom Torizon OS image with TorizonCore Builder, push it to Torizon Cloud and trigger a remote update to test the customized image.

### 2.1 Installing TorizonCore Builder

In the host, run the following commands to install TorizonCore Builder:

```default
$ mkdir -p /opt/labs/ex/tcb/ && cd /opt/labs/ex/tcb/
$ wget https://raw.githubusercontent.com/toradex/tcb-env-setup/master/tcb-env-setup.sh
$ source tcb-env-setup.sh -a local
```

Test the installation:

```default
$ torizoncore-builder --version
torizoncore-builder 3.8.1
```

Obs.: If you close the terminal, be aware that you will need to run the *source* command again.

### 2.2 Creating a custom Torizon OS image

In the host, run the following commands to clone a repository with the initial configuration to create a customized Torizon OS image:

```default
$ cd /opt/labs/ex/tcb/
$ git clone https://github.com/sergioprado/torizoncore-custom-image.git
$ cd torizoncore-custom-image/
```

Copy to this repository the Torizon OS image that will be used as input for the customization:

```default
$ mkdir -p images/
$ cp /opt/labs/dl/torizon-core-docker-verdin-imx8mp-Tezi_6.3.0+build.4.tar images/
```

Inspect the contents of the repository and the provided configuration file (*tcbuild.yaml*).

```default
$ tree
$ cat tcbuild.yaml
```

Change the configuration file to apply the customizations described below. For this, you will need to add a *customization* session in the file with the required changes (in case you need some guidance, check the [slides](https://e-labworks.com/training/en/tor/slides) or the [online documentation](https://developer.toradex.com/torizon/os-customization/torizoncore-builder-tool-build-command#customization-section)).

- Apply a splash screen (*media/splash-screen.png*).
- Apply a device tree overlay (*device-tree/enable-leds.dts*).
- Add a kernel command line argument *loglevel=3*.
- Add the content of the *rootfs-overlay* directory to the rootfs.

Now build the image:

```default
$ torizoncore-builder build --force --file tcbuild.yaml
```

### 2.3 Pushing the custom image to Torizon Cloud

Go to the Torizon Cloud's dashboard, enter "My Account" and click on "*Download Credentials*" to download the credentials file (*credentials.zip*).

Save the credentials file inside the directory where you have the image customizations (*/opt/labs/ex/tcb/torizoncore-custom-image/*).

Use TorizonCore Builder to push the customized image to Torizon Cloud (this command might take a few minutes to execute):

```default
$ torizoncore-builder platform push --credentials credentials.zip \
        --package-name "my-custom-image" --package-version "1.0" \
        --hardwareid verdin-imx8mp custom-image-branch
```

### 2.4 Updating the device with the customized image

In the target, run the command below to monitor the update process:

```default
# sudo journalctl -f -u aktualizr*
```

Go to Torizon Cloud's dashboard, select the device and click on "*Initiate Update*".

Execute the following steps to trigger an update to the customized image:

- Select "*verdin-imx8mp*" and click on "*Continue*".
- Select "*My Uploads*" and disable the other options.
- In the "*Select package*" field, choose "*my-custom-image*"
- In the "*Select version*" field, choose "*1.0*".
- Click on "*Continue*" and "*Finish*" to initiate the remote update process.

Monitor the update in the target. Since we just applied a few changes to the image, the update process should be fast.

### 2.5 Testing the customized image

After the update, let's check if the customizations in the image were successfully applied.

Confirm that *loglevel=3* was passed to the kernel command line:

```default
# cat /proc/cmdline
```

Check if the *secret.txt* file was added to the image:

```default
# sudo cat /etc/secret.txt
```

Test the access to the RGB LEDs enabled via device tree overlay:

```default
# cd /sys/class/leds/
# echo 1 > led-0/brightness && sleep 2 && echo 0 > led-0/brightness
# echo 1 > led-1/brightness && sleep 2 && echo 0 > led-1/brightness
# echo 1 > led-2/brightness && sleep 2 && echo 0 > led-2/brightness
```

## Lab 3 - Developing containerized applications

In this exercise, we will build a containerized web application that will provide a web page to turn on/off the LEDs from the carrier board.

We will learn how to containerize the application, push the image to a registry, test it on the target and create a package for remote updates via Torizon Cloud.

### 3.1 Creating a user on Docker Hub

For this exercise, you will need a user on Docker Hub. In case you don't have it, go to [https://hub.docker.com/](https://hub.docker.com/) and create one.

### 3.2 Creating a containerized web application

In the host, run the following commands to clone the repository with the source code of the web application:

```default
$ mkdir -p /opt/labs/ex/apps/ && cd /opt/labs/ex/apps/
$ git clone https://github.com/sergioprado/torizon-webapp.git
$ cd torizon-webapp
```

Inspect the contents of this repository. There is a configuration file for the web server (*lighttpd.conf*), a web page (*index.html*) and a CGI script to manipulate the LEDs (*leds.cgi*).

Now create a *Dockerfile* for this application:

- Use *linux/arm64* as the target platform and *alpine:3.17.1* as the base image.
- Run the *apk* command to install *lighttpd* and *lighttpd-mod_auth* packages.
- Run the *addgroup* command to create a group called *leds* with GID 44.
- Run the *addgroup* command to add the *lighttpd* user to the group *leds*.
- Copy the contents of *lighttpd/* to */etc/lighttpd/*.
- Copy the contents of *www/* to */var/www/localhost/*.
- Use the following command as the default command for the container.

```default
/usr/sbin/lighttpd -D -f /etc/lighttpd/lighttpd.conf
```

When you finish the implementation, compare it with the answer to make sure everything is correct.

Now run the following command to build the container (don't forget to replace *\<docker-hub-user\>* with your Docker Hub user name):

```default
$ docker build -t <docker-hub-user>/webapp:1.0 .
```

If the build is successful, you can push the image to Docker Hub. For that, you have to log in first:

```default
$ docker login
```

And then push the image to Docker Hub (replace *\<docker-hub-user\>* with your Docker Hub user name):

```default
$ docker push <docker-hub-user>/webapp:1.0
```

### 3.3 Testing the container in the device

In the target, execute the following command to run the container (replace *\<docker-hub-user\>* with your Docker Hub user name):

```default
# docker run -d --rm --name webapp -p 80:80 \
             -v /sys/class/leds/led-0/brightness:/sys/class/leds/led-0/brightness \
             -v /sys/class/leds/led-1/brightness:/sys/class/leds/led-1/brightness \
             -v /sys/class/leds/led-2/brightness:/sys/class/leds/led-2/brightness \
             <docker-hub-user>/webapp:1.0
```

Confirm that the container is running:

```default
# docker ps
```

Take note of the target's IP address:

```default
# ifconfig mlan0
```

In the host, open the web browser and access the IP address of the target. You should see a web page with buttons to manipulate the RGB LEDs from the carrier board. Have fun!

### 3.4 Creating and triggering an application update

Now that we have a containerized application, we can create a Compose file to push to Torizon Cloud, so we can do remote updates.

Based on the docker command used to execute the container, create a *docker-compose.yml* file. You might want to try the [composerize website](https://www.composerize.com) for that. When you finish the implementation, compare it with the answer to make sure everything is correct.

Now run the following command to push the Compose file to Torizon Cloud. Remember that, to run TorizonCore Builder, you have to first source the setup script (*tcb-env-setup.sh*). Also, you need to have the *credentials.zip* and *docker-compose.yml* files in the same directory you are running the command.

```default
$ torizoncore-builder platform push --credentials credentials.zip \
        --package-name webapp --package-version 1.0 \
        --canonicalize docker-compose.yml
```

Before trying a remote update of the application, run the following commands in the target to stop all containers, remove all images and monitor the update process:

```default
# docker container stop $(docker container ls -q) 2>&-
# docker system prune -a -f
# sudo journalctl -f -u aktualizr*
```

Go to the Torizon Cloud's dashboard, select the device and click on "*Initiate Update*".

Execute the following steps to trigger an application update:

- Select "*docker-compose*" and click on "*Continue*".
- Select "*My Uploads*" and disable the other options.
- In the "*Select package*" field, choose "*webapp*"
- In the "*Select version*" field, choose "*1.0*".
- Click on "*Continue*" and "*Finish*" to initiate the remote update process.

Monitor the update progress in the target's serial console. After the update is finished, check if the container is running:

```default
# docker ps
```

Open the web browser and access the IP address of the target to test the web application.

Reboot the device and test again. The container should automatically start at boot time.

## Lab 4 - Application development with VS Code

In this exercise, we will learn how to use Visual Studio Code with the Torizon extension to write, debug, and push to a registry a containerized application for Torizon.

### 4.1 Starting Visual Studio Code

In the host, run the following commands to start Visual Studio Code:

```default
$ mkdir -p /opt/labs/ex/apps && cd /opt/labs/ex/apps
$ code .
```

This instance of Visual Studio Code already has the Torizon extension and its dependencies installed.

### 4.2 Connecting to the device

Run the following command in the target and take note of its IP address:

```default
# ifconfig mlan0
```

In Visual Studio Code, access the Activity Bar (a vertical bar on the left with a few icons) and select the "*ApolloX Torizon*" extension (the one with the Torizon logo).

On the right of the Activity Bar, there is the Side Bar with a view labeled "*Network Devices*" listing all Torizon devices available for connection. Execute the following steps to connect to your device:

- Click on the '+' sign.
- Type the IP address of the target.
- When asked for the user, type *torizon*.
- Type the password you configured in the module.

After a few seconds, the device should appear in the Side Bar, under the "*Connected Devices*" view.

Select the device and click on "*Set Default*" to set the device as the default Docker Host. You should now have a view of the images and containers running in the device.

### 4.3 Creating a new C/C++ application

Press *Control+Shift+P*, type *torizon* and select "*ApolloX Torizon: Create new single container project*".

Select the "*C++ Console Application*" template and configure the project:

- Type *blinkled* in the "*Name*" field.
- Type *blinkled* in the "*Container name*" field.
- Type */opt/labs/ex/apps* in the "*Creation Path*" field.
- Click on "*Create Project*".

Visual Studio Code will switch the perspective to the newly created project.

Explore the contents of the project.

### 4.4 Writing the *blinkled* application

In the Project Explorer view, expand *src* and open *main.cpp*.

Write a simple C application to blink the LED:

- Use the *open()* function to open the LED file (*/sys/class/leds/led-3/brightness*).
- Use the *write()* function to write to the file.
  - Writing '1' will turn on the LED.
  - Writing '0' will turn off the LED.
- Use the *usleep()* for the delay (in microseconds).

In case you need help understanding how these functions work, have a look at their man pages:

```default
$ man 2 open
$ man 2 write
$ man 3 usleep
```

As soon as you have an initial version of the application, try to debug it:

- Put a breakpoint at the beginning of the code.
- Press *Control+Shift+D*.
- In the "*Run and Debug*" view, select "Torizon ARMv8".
- Press *F5*.

This will trigger the generation of a container with the cross-compiled application for the target, deploy the container to the device and start a remote debugging session! It might take a few minutes the first time you do this, but the next iterations will be faster.

During the debugging session, you might realize that the application is not able to open the LED file. This is because the application doesn't have access to it, since we didn't bind-mounted the LED file inside the container yet.

Press *Control+Shift+E* to go to the Project Explorer view, open the *docker-compose.yml* file and configure the bind mounts:

```diff
  version: "3.9"
  services:
    blinkled-debug:
      build:
        context: .
        dockerfile: Dockerfile.debug
      image: ${LOCAL_REGISTRY}:5002/blinkled-debug:${TAG}
      ports:
        - 2230:2230
+     volumes:
+       - /sys/class/leds/led-3/brightness:/sys/class/leds/led-3/brightness

    blinkled:
      build:
        context: .
        dockerfile: Dockerfile
      image: ${DOCKER_LOGIN}/blinkled:${TAG}
+     volumes:
+       - /sys/class/leds/led-3/brightness:/sys/class/leds/led-3/brightness
```

Try again. Now the application should be able to access the LED file.

Spend some time writing and debugging the application until the LED is blinking!

### 4.5 Pushing the application to a registry

In Visual Studio Code, to push the container image to a registry, execute the following steps:

- Press *Control+Shift+E* to open the Project Explorer view.
- On the Side Bar, search for the "Task runner" view and expand it.
- Click on "Create Production Docker Image".
- Type your Docker Hub's username in the "*Docker namespace*" field.
- Type *1.0* for the container tag.
- Type your Docker Hub's password.
- Select *arm64* for the container architecture.

A production version of the container image will be built and pushed to Docker Hub. In the Visual Studio Code terminal, you should see the message: "*Successfully Finished*".

### 4.6 Creating a Compose file for remote updates

Now create a Compose file that includes both the *blinkled* and the *webapp* (developed in Lab 3) containerized applications.

Use TorizonCore Builder to push the new Compose application to Torizon Cloud.

Before trying a remote update, run the following command in the target to stop any containers executed by Visual Studio Code during application debugging:

```default
# docker container stop $(docker container ls -q) 2>&-
```

Go to the Torizon Cloud's dashboard and trigger an update.

After the update, both containers should be running in the device. Reboot the module and confirm that the containerized applications are executed automatically at startup.

## Appendix A: Flashing Torizon OS with Easy Installer

### Configuring the device in recovery mode

Connect the target to the host machine using the two USB cables.

With the device powered on, hold the *Recovery* button, press *Reset* and release the *Recovery* button.

In VirtualBox, go to "*Devices*" -> "*USB*" and enable the device named "*NXP Semiconductors Inc*".

Run the following command to make sure the device is in recovery mode:

```default
$ lsusb | grep NXP
Bus 001 Device 011: ID 1fc9:0146 NXP Semiconductors SE Blank 865
```

### Starting the recovery program

In the host, uncompress Easy Installer and start the recovery program:

```default
$ cd /opt/labs/tools/
$ tar xfv /opt/labs/dl/tezi.tar.bz2
$ cd tezi/
$ ./recovery-linux.sh
```

After a few seconds, you should see the message "*Successfully downloaded Toradex Easy Installer*". In case the process doesn't complete, go to "*Devices*" -> "*USB*" in VirtualBox and make sure "FSL USB download gadget" is selected.

### Connecting to Easy Installer via VNC

In VirtualBox, go to "*Devices*" -> "*USB*" and make sure "Silicon Labs CP2105 Dual USB to UART Bridget Controller" and "Toradex Verdin iMX8M Plus Quad 4GB Wi-Fi / BT IT" are selected.

Run the following command to start the VNC client:

```default
$ vncviewer 192.168.11.1
```

You should now be able to see the Easy Installer graphical interface.

### Flashing TorizonCore 6.2.0

Connect to the module the pendrive provided by the instructor (this pendrive has the TorizonCore 6.2.0 image in it).

Select "*TorizonCore 6.2.0*". Make sure to select the correct version. Click on "*Install*" and confirm the installation.

A license agreement from NXP will be displayed. Click on "*I Accept*" and wait for the installation to finish.

At the end of the installation process, click on "*Reboot*" and close the VNC client.

Run the following command to access the target's serial console and see the logs from Torizon OS:

```default
$ minicom
```

After a few seconds, you should get a prompt to log in. If you cannot see a prompt, try typing *ENTER*.

To log in, the default user is *torizon* and the default password is *torizon*. As soon as you log in for the first time, you will be asked to change the password.
