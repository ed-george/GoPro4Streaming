#Mac Guide

Note: Much of this information was gained from [this Reddit thread](https://www.reddit.com/r/gopro/comments/2md8hm/how_to_livestream_from_a_gopro_hero4/coi41hg) and the excellent work of both [/u/3v1n0](https://www.reddit.com/r/gopro/comments/2md8hm/how_to_livestream_from_a_gopro_hero4/coi41hg) and @KonradIT 

##Step 1
In order to get the video to play on your machine, you will first need to connect your Mac to the GoPro through the GoPro's wireless network. Putting your GoPro into wireless mode (pressing and holding the left button for 2 seconds), should make the GoPro appear in the Wifi list of your Mac.

The device's generated SSID may come in the format `GPXXXXXXXXXXX` where `X` is a random identifier.

It should be noted that, by default, the WPA-2 pass for the GoPro network is `goprohero`, although the SSID and password setting can be changed through the GoPro app. This is recommended to ensure a secure streaming process.

For the next steps, until otherwise mentioned, you will need regular internet access and to not be connected to the GoPro's network.


##Step 2
In order to view the live video, you should install `ffmpeg` and `ffplay` on your Mac.

The quickest and easiest method of this is through the [Homebrew](http://brew.sh/) package manager.

```
brew install ffmpeg --with-ffplay
```

This will install the relevant libraries and make them immediately accessible from the Terminal

##Step 3
Connect your Mac to the GoPro's broadcasting network and run the `gopro_hero4.py` script in a Terminal window.

```
python gopro_hero4.py
``` 

This script is used as a [fix to intermittent streaming loss](https://www.reddit.com/r/gopro/comments/2md8hm/how_to_livestream_from_a_gopro_hero4/coi41hg) and was created by Reddit user [/u/3v1n0](https://www.reddit.com/user/3v1n0). 


##Step 4

Whilst connected to the GoPro via it's network, visit [http://10.5.5.9/gp/gpExec?p1=gpStreamA9&c1=restart](http://10.5.5.9/gp/gpExec?p1=gpStreamA9&c1=restart) in your browser.

The expected response should be:

```
{
  status: "0"
}
```
Should an error occur, try visiting [http://10.5.5.9/gp/gpControl](http://10.5.5.9/gp/gpControl) and locating the relevant stream restart URL. 

Firmware updates have previously changed this URL and thus may be subject to further such changes in the future.

##Step 5
In an additional Terminal window to the Python script, run the following:

```
ffplay -fflags nobuffer -f:v mpegts -probesize 8192 udp://:8554
```

The video from the `udp://:8554` source should then appear in an `ffplay` window