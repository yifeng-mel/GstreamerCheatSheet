## Install Gstreamer on MAC OS
```
brew install gstreamer gst-plugins-base gst-plugins-good gst-plugins-bad gst-plugins-ugly gst-libav
```
gst-libav provides avdec_vp8 plugin

## Video Streaming 
https://developer.ridgerun.com/wiki/index.php?title=Jetson_Nano/Gstreamer/Example_Pipelines/Streaming

Sender: \
CLIENT_IP=<IP_ADDRESS>
```
gst-launch-1.0 -e nvarguscamerasrc ! 'video/x-raw(memory:NVMM), width=2040, height=1080, framerate=20/1' ! omxvp8enc ! rtpvp8pay mtu=1400 ! udpsink host=10.0.0.25 port=5000 sync=false async=false
```

Receiver:
```
gst-launch-1.0 udpsrc port=5000 ! application/x-rtp,encoding-name=VP8,payload=96 ! rtpvp8depay ! queue ! avdec_vp8 ! glimagesink sync=false async=false
```

## Video Recording
https://forums.developer.nvidia.com/t/how-capture-video-from-camera-on-jetson-nano-in-h264/80180

```
gst-launch-1.0 nvarguscamerasrc ! 'video/x-raw(memory:NVMM),width=2040, height=1080, framerate=20/1, format=NV12' ! omxh264enc ! qtmux ! filesink location=test.mp4 -e
```
```
gst-launch-1.0 nvarguscamerasrc num-buffers=1200 gainrange="1 1" ispdigitalgainrange="2 2" ! 'video/x-raw(memory:NVMM),width=1920, height=1080, framerate=30/1, format=NV12' ! omxh264enc ! qtmux ! filesink location=test.mp4 -e
```

