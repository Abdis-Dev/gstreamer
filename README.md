Note :

1. Please make sure "gstreamer-1.0-msvc-x86_64-1.16.2" and "gstreamer-1.0-devel-msvc-x86_64-1.16.2" installed correctly (Choose "COMPLETE" version when installing gstreamer).
2. Add installation to system windows path. 

> Computer -> System properties -> Advanced System Settings -> Advanced Tab -> Environment Variables... -> System Variables -> Variable: Path -> Edit -> New -> C:\gstreamer\1.0\x86_64\bin

> New Variable: GST_SDK_PATH= C:\gstreamer\1.0\x86_64\

3. Choose Windows 64-bit for Target Platform so the program can be run properly.

For streaming from command prompt (CMD as streamer and Unity as listener) :
1. Open and run scene "listenerCamera"
2. Run this following command in cmd, change "Integrated Camera" to your camera name (check the name on device manager).
> gst-launch-1.0 ksvideosrc device-name="Integrated Camera" ! videoconvert ! videoscale ! video/x-raw,format=I420,width=1280,height=720,framerate=30/1 ! videoconvert ! x264enc name=videoEnc bitrate=2000 tune=zerolatency pass=qual ! rtph264pay ! udpsink host=127.0.0.1 port=7000 sync=false -v
3. Unity window should be active (can not be minimize or the program will be paused).

For streaming  and listening from command prompt (CMD as streamer and CMD as listener) :
1. Open and run scene "streamer"
2. Run this following command in cmd.
> gst-launch-1.0 udpsrc port=7000 ! application/x-rtp ! rtpjitterbuffer ! rtph264depay ! decodebin ! videoflip method=5 ! autovideosink sync=false -v
3. Unity window should be active (can not be minimize or the program will be paused).

Quick Streming low delay:
gst-launch-1.0 ksvideosrc device-index=0 ! videoconvert ! video/x-raw,format=I420 ! x264enc name=videoEnc bitrate=10000 tune=zerolatency pass=qual  speed-preset=ultrafast ! rtph264pay config-interval=-1 ! udpsink host=10.24.80.127 port=7000 sync=false -v

