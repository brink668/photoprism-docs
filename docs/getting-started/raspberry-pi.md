# Running PhotoPrism on a Raspberry Pi

Our [stable version](../release-notes.md) and development preview now come as a single
[multi-arch image](https://hub.docker.com/r/photoprism/photoprism) for AMD64, ARM64, and ARMv7. If your device meets the system requirements, mostly the same [installation instructions](docker-compose.md)
as for regular Linux servers apply.

!!! info ""
    Use `photoprism/photoprism:latest`for the stable release or `photoprism/photoprism:preview` for testing 
    preview builds. Make sure to [pull the most recent image](updates.md) from Docker Hub. Existing users are advised to 
    update their `docker-compose.yml` config based on our examples for [ARM64](https://dl.photoprism.org/docker/arm64/docker-compose.yml)
    and [ARMv7](https://dl.photoprism.org/docker/armv7/docker-compose.yml)-based devices.

!!! missing ""
    Owners of [ARMv7-based devices](https://dl.photoprism.org/docker/armv7/docker-compose.yml) have to revert to 
    an [alternative image](https://hub.docker.com/r/linuxserver/mariadb) if they want to use MariaDB. 
    The [official image](https://hub.docker.com/_/mariadb) is available for AMD64 and ARM64 only. 
    Pay close attention to changed directory and environment variable names.

### System Requirements ###

It's important that you [boot](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) 
your Raspberry Pi 3 / 4 with the parameter `arm_64bit=1` in `config.txt` to use our ARM64 image.
Alternatively, you may run it on [UbuntuDockerPi](https://github.com/guysoft/UbuntuDockerPi).
It's a 64bit Ubuntu Server with Docker pre-installed.

Your device should have at least 4 GB of memory. Also make sure it has at least
4 GB of [swap](https://opensource.com/article/18/9/swap-space-linux-systems)
configured, so that indexing doesn't cause restarts when there are memory usage spikes.

!!! note ""
    Indexing large photo and video collections significantly benefits from fast, local SSD storage,
    and plenty of memory for caching. Especially the conversion of RAW images and the transcoding of
    videos are very demanding.

To avoid permission issues, your [docker-compose.yml](https://dl.photoprism.org/docker/arm64/docker-compose.yml) 
should include the following security options:

```yaml
  photoprism:
    security_opt:
      - seccomp:unconfined
      - apparmor:unconfined
```

Big thank you to [Guy Sheffer](https://github.com/guysoft) for
[building](https://github.com/photoprism/photoprism/issues/109) this!

!!! info ""
    If you're running out of memory - or other system resources - while indexing, try reducing the
    [number of workers](https://docs.photoprism.org/getting-started/config-options/) by setting
    `PHOTOPRISM_WORKERS` to a reasonably small value in `docker-compose.yml` (depending on the performance of the server).
    As a measure of last resort, you may disable using TensorFlow for image classification and facial recognition.
