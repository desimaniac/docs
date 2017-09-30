# Enable iGPU on Hetzner


## iGPU not loaded


iGPU not loaded as seen with:
```
$ ls -la /dev/dri
ls: cannot access '/dev/dri': No such file or directory
```

vainfo:

```
$ vainfo
error: can't connect to X server!
error: failed to initialize display
Aborted (core dumped)
```




See if iGPU is supported on your server:


```
sudo lspci -v -s $(lspci | grep VGA | cut -d" " -f 1)

```

Look for `Kernel modules: i915`, however, `Kernel driver in use: i915` will be missing. 


## Steps to enable iGPU

Hetzner disables loading of video card drivers by default until login to the desktop, which you never do when running a server. They also seem to blacklist i915 drivers. To get VA-API to work on Hetzner, you have to make the following changes:

Comment all line referencing i915 in `/etc/modprobe.d/blacklist-hetzner.conf`:

```shell
$ cat /etc/modprobe.d/blacklist-hetzner.conf
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


Hetzner's default grub config blocks loading of video card drivers. Change `GRUB_CMDLINE_LINUX_DEFAULT="nomodeset net.ifnames=0"` to `GRUB_CMDLINE_LINUX_DEFAULT="net.ifnames=0"` in `/etc/default/grub.d/hetzner.cfg`.

```shell
$ cat /etc/default/grub.d/hetzner.cfg
GRUB_HIDDEN_TIMEOUT_QUIET=false
GRUB_CMDLINE_LINUX_DEFAULT="net.ifnames=0"

# only use text mode - other modes may scramble screen
GRUB_GFXPAYLOAD_LINUX="text"
```

Reload grub and reboot.
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
sudo reboot

```


## Verifying iGPU is enabled


```
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






Look for `Kernel driver in use: i915`:

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


```
$ ls -la /dev/dri
total 0
drwxr-xr-x  2 root root        80 Sep 30 05:14 .
drwxr-xr-x 20 root root      4200 Sep 30 05:14 ..
crw-rw----  1 root video 226,   0 Sep 30 05:14 card0
crw-rw----  1 root video 226, 128 Sep 30 05:14 renderD128
```


Sources:
https://www.reddit.com/r/seedboxes/comments/57uq5e/hardware_video_encoding_on_hetzner_server_with/
