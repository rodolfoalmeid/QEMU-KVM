 QEMU-KVM
 ===========
 
## Ubuntu Firewall Blocking KVM Netowrk
 
The technique to use to make this work on 22.04 was to edit /etc/ufw/sysctl.conf to have:
> Don't filter packets to our libvirt guests
net.bridge.bridge-nf-call-ip6tables = 0
net.bridge.bridge-nf-call-iptables = 0
net.bridge.bridge-nf-call-arptables = 0

The problem is on later releases of Ubuntu these are not available until the 'bridge' module is loaded. ufw startup happens extremely early in the boot process-- intentionally before networking comes up. If the bridge module is loaded after ufw starts, it set the sysctl values to its default values-- ie, all '1', which breaks libvirt.
