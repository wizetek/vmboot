### vmboot
## Description
QEMU VM launch wrapper - BASH shell script

## Example usage
* Boot Linux ISO
```
$ vmboot endeavouros-2021.08.27-x86_64.iso
```
output:
```
qemu-system-x86_64 -accel kvm -cpu host -smp 2 -m 4G -nic mac=52:54:1f:cd:de:9a -device AC97 -cdrom "endeavouros-2021.08.27-x86_64.iso"
```
* Boot Linux hard disk image
```
$ vmboot archman-20G.raw 
```
output:
```
qemu-system-x86_64 -accel kvm -cpu host -smp 2 -m 4G -nic mac=52:54:d9:2e:e9:01 -device AC97 -drive format=raw,file="archman-20G.raw"
```
* Boot Windows® ISO + hard disk image, Realtek network card, 8 GB RAM
```
$ vmboot win7sp1_PRO_x64FRE_en-us.iso -hda 40G.qcow2 -nic model=rtl8139 -m 8G
```
output:
```
qemu-system-x86_64 -accel kvm -cpu host -smp 2 -device AC97 -hda 40G.qcow2 -nic model=rtl8139 -m 8G -cdrom "win7sp1_PRO_x64FRE_en-us.iso"
```

## Help
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
  -nic mac=52:54:01:23:45:67
  -nic bridge		(Must be configured on host)
  -nic none		No network
  -nic hostfwd=tcp::5022-:22
  -nic hostfwd=tcp::6001-:6000
  -nic hostfwd=tcp::5555-:3389
  -nic model=virtio-net-pci
  -nic model=?		List supported NIC models
  -soundhw ?		List supported sound cards
  -device ?		List all available devices by type
  -device usb-tablet	(Prevents mouse grab)
  -device VGA,edid=on,xres=1366,yres=768
  -loadvm <tag>		Load saved state from snapshot
  -snapshot		Don't write changes to disk image (can still 'commit all' in Monitor)
  -S			Start paused
  -no-shutdown		Don't exit on shutdown
  -no-quit		Don't allow closing window
  -full-screen
  -name			Title for QEMU window and VNC window
  -vnc :1		Video output to VNC server display :1 (password set in Monitor)
  -vnc :0,to=9,lossy=on	Try :0, :1, :2, :3, ... until :9; use lossy JPEG compression
  -display none		No video output to user (guest still has video)
  -nographic		No GUI / command line only (output and Monitor in host console)
  -monitor stdio	Monitor in host console
  -monitor unix:/tmp/qemu-monitor,server,nowait		('socat /tmp/qemu-monitor STDIO')
  -monitor telnet::5023,server,nowait
  -daemonize		Detach process from stdio

For more QEMU options type: vmboot - --help
```

## Configuration file
automatically generated
```
# ~/.config/vmbootrc
#
# vmboot configuration file
#
# CMD points to QEMU binary
# CPU is recommended for best performance
# SMP, RAM, NIC, SND, OPT are optional

CMD="qemu-system-x86_64"
CPU="-accel kvm -cpu host"
# Newer Q35 PCIe instead of i440FX PCI host bridge
#CPU="-machine q35,accel=kvm -cpu host"
SMP="-smp 2"
RAM="-m 4G"
#NIC="-nic mac=52:54:01:23:45:67"
# Random MAC address
NIC="-nic mac=$( printf 52:54:%02x:%02x:%02x:%02x $(( ${RANDOM} & 0xff )) $(( ${RANDOM} & 0xff )) $(( ${RANDOM} & 0xff)) $(( ${RANDOM} & 0xff )) )"
#SND="-soundhw hda"
SND="-device AC97"
#OPT="-monitor unix:/tmp/qemu-monitor,server,nowait -action panic=pause"
```
