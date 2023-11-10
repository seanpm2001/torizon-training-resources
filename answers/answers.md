# Answers - Torizon Bootcamp

|                   |                     |
| :---------------- | :------------------ |
| **Target**        | Verdin iMX8MP       |
| **Language**      | English (US)        |
| **Version**       | 2.0.0               |

- [Labs 1](#labs-1)
  - [1.1](#11)
  - [1.2](#12)
  - [1.3](#13)
  - [1.4](#14)
  - [1.5](#15)
  - [1.6](#16)
- [Labs 2](#labs-2)
  - [2.1](#21)
  - [2.2](#22)
  - [2.3](#23)
  - [2.4](#24)
  - [2.5](#25)
- [Labs 3](#labs-3)
  - [3.1](#31)
  - [3.2](#32)
  - [3.3](#33)
  - [3.4](#34)
- [Labs 4](#labs-4)
  - [4.1](#41)
  - [4.2](#42)
  - [4.3](#43)
  - [4.4](#44)
  - [4.5](#45)
  - [4.6](#46)

## Labs 1

### 1.1

\-

### 1.2

\-

### 1.3

\-

### 1.4

\-

### 1.5

\-

### 1.6

\-

## Labs 2

### 2.1

\-

### 2.2

```diff
        variant: torizon-core-docker
        build-number: "17"

+customization:
+  splash-screen: media/splash-screen.png
+  device-tree:
+    overlays:
+      add:
+        - device-tree/enable-leds.dts
+  kernel:
+    arguments:
+      - loglevel=3
+  filesystem:
+     - rootfs-overlay

  output:
    ostree:
```

### 2.3

\-

### 2.4

\-

### 2.5

\-

## Labs 3

### 3.1

\-

### 3.2

```Dockerfile
FROM --platform=linux/arm64 alpine:3.17.1

# install lighttpd packages
RUN apk add --update --no-cache \
    lighttpd lighttpd-mod_auth && \
    rm -rf /var/cache/apk/*

# create leds group and add lighttpd user to it
RUN addgroup -g 44 leds && \
    addgroup lighttpd leds

# copy custom lighttpd configuration
COPY lighttpd/ /etc/lighttpd/

# copy web-related files
COPY www/ /var/www/localhost/

# command to run when starting the container
CMD ["/usr/sbin/lighttpd", "-D", "-f", "/etc/lighttpd/lighttpd.conf"]
```

### 3.3

\-

### 3.4

```yaml
version: '3.3'

services:
  webapp:
    image: <docker-hub-user>/webapp:1.0
    ports:
      - 80:80
    volumes:
      - /sys/class/leds/led-0/brightness:/sys/class/leds/led-0/brightness
      - /sys/class/leds/led-1/brightness:/sys/class/leds/led-1/brightness
      - /sys/class/leds/led-2/brightness:/sys/class/leds/led-2/brightness
```

## Labs 4

### 4.1

\-

### 4.2

\-

### 4.3

\-

### 4.4

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>

#define BLINK_DELAY  500000

const char *led_file = "/sys/class/leds/led-3/brightness";
const char *led_on   = "1";
const char *led_off  = "0";

int main()
{
    int fd;

    fd = open(led_file, O_WRONLY);
    if (fd == -1) {
        printf("Error opening the LED file! [%s]\n", strerror(errno));
        return 1;
    }

    while(1) {
        write(fd, led_on, strlen(led_on));
        usleep(BLINK_DELAY);

        write(fd, led_off, strlen(led_off));
        usleep(BLINK_DELAY);
    }

    close(fd);

    return 0;
}
```

### 4.5

\-

### 4.6

```yml
version: '3.3'

services:
  webapp:
    image: <docker-hub-user>/webapp:1.0
    ports:
      - 80:80
    volumes:
      - /sys/class/leds/led-0/brightness:/sys/class/leds/led-0/brightness
      - /sys/class/leds/led-1/brightness:/sys/class/leds/led-1/brightness
      - /sys/class/leds/led-2/brightness:/sys/class/leds/led-2/brightness
  blinkled:
    image: <docker-hub-user>/blinkled:1.0
    volumes:
      - /sys/class/leds/led-3/brightness:/sys/class/leds/led-3/brightness
```

```default
$ torizoncore-builder platform push --credentials credentials.zip \
        --package-name finalapp --package-version 1.0 \
        --canonicalize --force docker-compose.yml
```
