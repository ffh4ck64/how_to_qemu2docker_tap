# How to connecting qemu to docker custom bridge via tap iface?

### 1. Enable tap kernel module.
```
$ sudo modprobe tap
```

### 2. Create the tap interface througth ip, it is Debian util. (at root)
```
$ ip tuntap add dev d2q-tap0 mode tap
$ ip link set d2q-tap0 master br-...
$ ip link set d2q-tap0 up
```

There \<br-...\> is your custom docker iface.

### 3. Run qemu with custom network setup. (at root)
```
$ qemu-system-x86_64 \
    -hda kali.qcow2 -boot a \
    -netdev tap,id=net0,ifname=d2q-tap0,script=no,downscript=no \
    -device e1000,netdev=net0
```

It's done. Happy hacking!
