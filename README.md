# Docker Video Processing

[![ffmpeg](https://img.shields.io/badge/alpine-v3.4.6-blue.svg)](https://alpinelinux.org)
[![ffmpeg](https://img.shields.io/badge/python-v3.6.1-blue.svg)](https://www.python.org)
[![ffmpeg](https://img.shields.io/badge/ffmpeg-v3.3.2-blue.svg)](https://ffmpeg.org)
[![ffmpeg](https://img.shields.io/badge/opencv-v3.2.0-blue.svg)](http://opencv.org)

Provides a Docker image intended for video processing.

It includes:
- [Python](https://www.python.org)
- [FFMPEG](https://ffmpeg.org)
- [OpenCV](http://opencv.org)

FFMPEG and OpenCV are compiled from sources.

## Installation
```
$ docker pull alexiskofman/video-processing:latest
```

Then enjoy what's inside:
```
$ docker run --rm -ti alexiskofman/video-processing:latest ash
```

# License

MIT Licensed. Copyright (c) Alexis Kofman 2017.
