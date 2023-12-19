# rpi-hotspot-mjpg-streamer

It supports the following architectures automatically:

- amd64
- arm/v6
- arm/v7
- arm64

## Tested devices

| Device              | Working? |
| ------------------- | -------- |
| amd64               | Untested |
| Raspberry Pi 2b     | Untested |
| Raspberry Pi 3b+    | ✅       |
| Raspberry Pi 4b     | ✅       |
| Raspberry Pi Zero W | Untested |

Please let me know if you test any others, would love to increase the compatibility list!

## Usage

Interactive

```shell
docker run -it --rm \
  --device=/dev/video0 \
  -p 8080:8080 \
  2kita/mjpg-streamer
```

Docker-Compose.yml

```yaml
mjpg-streamer:
   restart: always
   image: 2kita/mjpg-streamer
   devices:
     - /dev/video0
   ports:
     - 8080:8080
   environment:
     - MJPEG_STREAMER_INPUT="-y -n -r 640x480"
```

## Environment Variables

| Variable                 | Description                    | Default Value      |
| ------------------------ | ------------------------------ | ------------------ |
| CAMERA_DEV               | The camera device node         | `/dev/video0`      |
| MJPEG_STREAMER_INPUT     | Flags to pass to mjpg_streamer | `-y -n -r 640x480` |

## Webcam integration

## USB Webcam

1. Bind the camera to the docker using --device=/dev/video0:/dev/videoX
2. Optionally, change `MJPEG_STREAMER_INPUT` to your preferred settings (ex: `input_uvc.so -y -n -r 1280x720 -f 10`)

## Octoprint integration

Use the following settings in octoprint:

```yaml
webcam:
  stream: http://name-of-your-docker-host:8080/?action=stream
  snapshot: http://name-of-mjpg-streamer-container:8080/?action=snapshot
  ffmpeg: /opt/ffmpeg/ffmpeg
```

The steam URL needs to be able to connect from your web browser but the snapshots can connect over the internal docker network. If you are using the official Octoprint container, the path to the `ffmpeg` binary is `/opt/ffmpeg/ffmpeg`.