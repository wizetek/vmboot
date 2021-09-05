# vmboot
QEMU VM launch wrapper
```
$ vmboot -h

Basic usage:

  vmboot diskimage.img

QEMU options used automatically based on file extension:
  -cdrom	for .iso
  -hda		for .qcow2 .vdi .vmdk .vhdx .img ...
  -drive file=	for .raw

For advanced usage type: vmboot --help
```
```
$ vmboot --help

  vmboot [-|-raw|-cd] diskimage.img [-opt] [-opt] [...]

Optional first switch:
  -		Pass everything that follows directly to QEMU
  -raw		Force raw format for disk image
  -cd		Force cdrom medium for disk image

Some common QEMU [-opt]ions:
  -boot d		a b (FDD1 FDD2), c d (HDD1 HDD2), d (CDROM), n (NIC)
  -boot c,once=d
  -nic hostfwd=tcp::5022-:22
  -nic hostfwd=tcp::6001-:6000
  -nic hostfwd=tcp::5555-:3389
  -nic bridge,mac=52:54:00:12:34:56
  -nic model=virtio-net-pci
  -nic none		No network
  -nic,model=?		List supported NIC models
  -soundhw ?		List supported sound cards
  -device ?		List all available devices by type
  -device usb-tablet	(Prevents mouse grab)
  -device VGA,edid=on,xres=1366,yres=768
  -loadvm <tag>		Load saved state from snapshot
  -snapshot		Don't write changes to disk image (can still 'commit all' in Monitor)
  -S			Start paused
  -no-shutdown		Don't exit on shutdown
  -no-quit		Don't allow closing window
  -vnc :1		Video output to VNC server display :1 (password set in Monitor)
  -vnc :0,to=9,lossy=on	Try :0, :1, :2, :3, ... until :9; use lossy JPEG compression
  -display none		No video output to user (guest still has video)
  -nographic		No GUI / command line only (output and Monitor in host console)
  -monitor stdio	Monitor in host console
  -monitor telnet:127.0.0.1:5023,server,nowait
  -daemonize		Detach process from stdio
  -name			Set name for window title and VNC

For more QEMU options type: vmboot - --help
```
---
~/.config/vmbootrc
```
# /home/user/.config/vmbootrc
#
# vmboot configuration file
#
# CMD points to QEMU binary
# CPU is recommended for best performance
# SMP, RAM, NIC, SND, OPT are optional

CMD="qemu-system-x86_64"
CPU="-enable-kvm -cpu host"
SMP="-smp 2"
RAM="-m 4G"
#NIC="-nic mac=52:54:00:01:02:03"
# Random MAC address
NIC="-nic mac=$( printf 52:54:%02x:%02x:%02x:%02x $(( ${RANDOM} & 0xff )) $(( ${RANDOM} & 0xff )) $(( ${RANDOM} & 0xff)) $(( ${RANDOM} & 0xff )) )"
#SND="-soundhw hda"
SND="-device AC97"
#OPT="-action panic=pause"
```
