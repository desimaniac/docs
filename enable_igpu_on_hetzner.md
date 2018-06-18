# How to enable iGPU on Hetzner - For Hardware Accelerated Transcoding in Plex/Emby


Note: This was tested on Hetzner's EX41-SSD Server, running Intel Core i7-7700 CPU on Ubuntu Server 16.04, but should work on other Hetzner servers with supported Intel CPUs (see [this](https://support.plex.tv/hc/en-us/articles/115002178853-Using-Hardware-Accelerated-Streaming) and [this](https://en.wikipedia.org/wiki/Intel_Quick_Sync_Video)).

_Note: If you have a Coffee Lake or newer CPU (e.g. the Intel Core i7-8700 in EX61-NVME), you will need to either 1) update to Ubuntu Server 18.04, or 2) update the kernel on 16.04 (see [this](https://github.com/pimlie/ubuntu-mainline-kernel.sh)), to have /dev/dri show up._

## When iGPU isnt loaded...

`/dev/dri` path will be missing...

```
$ ls -la /dev/dri
ls: cannot access '/dev/dri': No such file or directory
```

`vainfo` will show this:

```
$ vainfo
error: can't connect to X server!
error: failed to initialize display
Aborted (core dumped)
```

Note: You may have to install `vainfo` via `sudo apt-get install vainfo`. 


## See if iGPU is supported on your server


Run the following command:
```bash
sudo lspci -v -s $(lspci | grep VGA | cut -d" " -f 1)

```

If your server supports iGPU, you will see something similar to `Kernel modules: i915`. However, kernel driver will not be loaded (i.e. `Kernel driver in use: i915` will be missing).


## Steps to enable iGPU

Hetzner disables loading of video card drivers unless install desktop OS. So to enable the loading of the drivers (ie. VA-AI) follow the steps below:

1. Open `/etc/modprobe.d/blacklist-hetzner.conf` (e.g. `sudo nano /etc/modprobe.d/blacklist-hetzner.conf`) and comment-out (add a `#` in front of) all lines referencing `i915` driver or similar. 

  
   Before:
   ```shell
   ### Hetzner Online GmbH - installimage
   ### silence any onboard speaker
   blacklist pcspkr
   blacklist snd_pcsp
   ### i915 driver blacklisted due to various bugs
   ### especially in combination with nomodeset
   blacklist i915 
   blacklist i915_bdw
   ### mei driver blacklisted due to serious bugs
   blacklist mei
   blacklist mei-me
   ```
  
  
   After:
   ```shell
   ### Hetzner Online GmbH - installimage
   ### silence any onboard speaker
   blacklist pcspkr
   blacklist snd_pcsp
   ### i915 driver blacklisted due to various bugs
   ### especially in combination with nomodeset
   #blacklist i915 
   #blacklist i915_bdw
   ### mei driver blacklisted due to serious bugs
   blacklist mei
   blacklist mei-me
   ```


2. Open `/etc/default/grub.d/hetzner.cfg` (e.g. `sudo nano /etc/default/grub.d/hetzner.cfg`) and delete `nomodeset` from the   `GRUB_CMDLINE_LINUX_DEFAULT` line.

   _Note: For Ubuntu 18.04, this file may just be `/etc/default/grub`._
  
   Before: 
   ```shell
   GRUB_HIDDEN_TIMEOUT_QUIET=false
   GRUB_CMDLINE_LINUX_DEFAULT="nomodeset net.ifnames=0"

   # only use text mode - other modes may scramble screen
   GRUB_GFXPAYLOAD_LINUX="text"
   ```   
   
   After: 
   ```shell
   GRUB_HIDDEN_TIMEOUT_QUIET=false
   GRUB_CMDLINE_LINUX_DEFAULT="net.ifnames=0"

   # only use text mode - other modes may scramble screen
   GRUB_GFXPAYLOAD_LINUX="text"
   ```

3. Reload grub and reboot.
   
   ```shell
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   sudo reboot
   ```


## Verify iGPU is enabled

`/dev/dri` will now exist with similar contents as below:

```
$ ls -la /dev/dri
total 0
drwxr-xr-x  2 root root        80 Sep 30 05:14 .
drwxr-xr-x 20 root root      4200 Sep 30 05:14 ..
crw-rw----  1 root video 226,   0 Sep 30 05:14 card0
crw-rw----  1 root video 226, 128 Sep 30 05:14 renderD128
```



`vainfo` will now show the details of the video driver.

```shell
$ vainfo
error: can't connect to X server!
libva info: VA-API version 0.39.0
libva info: va_getDriverName() returns 0
libva info: Trying to open /usr/lib/x86_64-linux-gnu/dri/i965_drv_video.so
libva info: Found init function __vaDriverInit_0_39
libva info: va_openDriver() returns 0
vainfo: VA-API version: 0.39 (libva 1.7.0)
vainfo: Driver version: Intel i965 driver for Intel(R) Kabylake - 1.7.0
vainfo: Supported profile and entrypoints

VAProfileMPEG2Simple            :	VAEntrypointVLD
VAProfileMPEG2Simple            :	VAEntrypointEncSlice
VAProfileMPEG2Main              :	VAEntrypointVLD
VAProfileMPEG2Main              :	VAEntrypointEncSlice
VAProfileH264ConstrainedBaseline:	VAEntrypointVLD
VAProfileH264ConstrainedBaseline:	VAEntrypointEncSlice
VAProfileH264Main               :	VAEntrypointVLD
VAProfileH264Main               :	VAEntrypointEncSlice
VAProfileH264High               :	VAEntrypointVLD
VAProfileH264High               :	VAEntrypointEncSlice
VAProfileH264MultiviewHigh      :	VAEntrypointVLD
VAProfileH264MultiviewHigh      :	VAEntrypointEncSlice
VAProfileH264StereoHigh         :	VAEntrypointVLD
VAProfileH264StereoHigh         :	VAEntrypointEncSlice
VAProfileVC1Simple              :	VAEntrypointVLD
VAProfileVC1Main                :	VAEntrypointVLD
VAProfileVC1Advanced            :	VAEntrypointVLD
VAProfileNone                   :	VAEntrypointVideoProc
VAProfileJPEGBaseline           :	VAEntrypointVLD
VAProfileJPEGBaseline           :	VAEntrypointEncPicture
VAProfileVP8Version0_3          :	VAEntrypointVLD
VAProfileVP8Version0_3          :	VAEntrypointEncSlice
VAProfileHEVCMain               :	VAEntrypointVLD
VAProfileHEVCMain               :	VAEntrypointEncSlice
VAProfileHEVCMain10             :	VAEntrypointVLD
VAProfileVP9Profile0            :	VAEntrypointVLD
VAProfileVP9Profile2            :	VAEntrypointVLD
```



You can also check by running `sudo lspci -v -s $(lspci | grep VGA | cut -d" " -f 1)` and finding `Kernel driver in use: i915`.

```
$ sudo lspci -v -s $(lspci | grep VGA | cut -d" " -f 1)
00:02.0 VGA compatible controller: Intel Corporation Device 5912 (rev 04) (prog-if 00 [VGA controller])
	Subsystem: Fujitsu Technology Solutions Device 121c
	Flags: bus master, fast devsel, latency 0, IRQ 125
	Memory at ee000000 (64-bit, non-prefetchable) [size=16M]
	Memory at d0000000 (64-bit, prefetchable) [size=256M]
	I/O ports at f000 [size=64]
	[virtual] Expansion ROM at 000c0000 [disabled] [size=128K]
	Capabilities: [40] Vendor Specific Information: Len=0c <?>
	Capabilities: [70] Express Root Complex Integrated Endpoint, MSI 00
	Capabilities: [ac] MSI: Enable+ Count=1/1 Maskable- 64bit-
	Capabilities: [d0] Power Management version 2
	Capabilities: [100] #1b
	Capabilities: [200] Address Translation Service (ATS)
	Capabilities: [300] #13
	Kernel driver in use: i915
	Kernel modules: i915
```


***



## Enable HW acceleration in Plex for [Cloudbox](https://cloudbox.rocks)

Hardware acceleration is currently only available to Plex Pass members. 

_Note: **ALL** the steps below are essential!_

1. Add the `plexpass` tag to the Cloudbox settings.yml, if it isn't already there (see [this](https://github.com/Cloudbox/Cloudbox/wiki/First-Time-Install:-Configuring-Settings)).


2. Update the Plex container (your database and settings will remain intact).

   ```shell
   sudo ansible-playbook cloudbox.yml --tags plex
   ```
   
   _Note: This step is important because it adds `/dev/dri/` to the Plex container. HW acceleration will not work without it._ 

3. Enable HW Acceleration in Plex: 

   - Settings -> Server -> Transcoder
   
   - "Use hardware acceleration when available": enabled


## Enable HW acceleration in Emby for [Cloudbox](https://cloudbox.rocks)


_Note: **ALL** the steps below are essential!_

1. Update the Emby container (your database and settings will remain intact).

   ```shell
   sudo ansible-playbook cloudbox.yml --tags emby
   ```
   
   _Note: This step is important because it adds `/dev/dri/renderD128` to the Emby container. HW acceleration will not work without it._ 

1. Enable HW Acceleration in Emby: 

   - Gear icon -> Server -> Transcoding
   
   - "Hardware acceleration:" `Video Acceleration API (VA API) (experimental)`
   
   - "VA API Device:" `/dev/dri/renderD128`
   
   - "Enable hardware encoding": enabled


***


## Tests


### Intel GPU Top tool

To install it: 

- `sudo apt-get install intel-gpu-tools`

To run it:

- `sudo intel_gpu_top`



### Test 1

CPU: [Intel Core i7-7700](https://ark.intel.com/products/97128/Intel-Core-i7-7700-Processor-8M-Cache-up-to-4_20-GHz) <br />
OS: Ubuntu 16.04 LTS <br />
Plex Server ([docker](https://github.com/plexinc/pms-docker)): 1.10.0.4523 <br />
Plex transcode: Movie @ 1080p BluRay Remux (H264) --> 4Mbps 720p HD (H264).

#### CPU Usage (`htop`)

Without HW Acceleration: ~450%

![](https://i.imgur.com/o1f4PUV.png)


With HW Acceleration: ~40%

![](https://i.imgur.com/mcrRDcK.png)


#### iGPU Usage (`intel_gpu_top`)

Without HW Acceleration:

![](https://i.imgur.com/SqQoij3.png)

With HW Acceleration:

![](https://i.imgur.com/NbzMP2q.png)



### Test 2


CPU: [Intel Core i7-7700](https://ark.intel.com/products/97128/Intel-Core-i7-7700-Processor-8M-Cache-up-to-4_20-GHz) <br />
OS: Ubuntu 16.04 LTS <br />
Plex Server ([docker](https://github.com/plexinc/pms-docker)): 1.10.0.4523 <br />
Plex transcode: Movie @ 2160p (4K-UHD) BluRay Remux (HEVC) --> 4Mbps 720p HD (H264).

#### CPU Usage (`htop`)

Without HW Acceleration: ~550%

![](https://i.imgur.com/l8xTmi8.png)


With HW Acceleration: ~40%

![](https://i.imgur.com/x2GknKc.png)


#### iGPU Usage (`intel_gpu_top`)

Without HW Acceleration:

![](https://i.imgur.com/7pNO5c8.png)

With HW Acceleration:

![](https://i.imgur.com/qHbNtLR.png)


---

<sub>References: </sub></br><sub>https://www.reddit.com/r/seedboxes/comments/57uq5e/hardware_video_encoding_on_hetzner_server_with/
</sub>
