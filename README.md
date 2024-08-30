# raspberrypi-CM4-M.2-Akey-Mkey-Bkey
raspberypi　CM4にpcie swich を載せてM.2基板を複数動作できる基板を作成しました。
wifi6　１G　2.5Gのnicが動きます。
MkeyにはNVMeを搭載しています。

$ lspci<br>
00:00.0 PCI bridge: Broadcom Inc. and subsidiaries BCM2711 PCIe Bridge (rev 20)<br>
01:00.0 PCI bridge: ASMedia Technology Inc. ASM1184e 4-Port PCIe x1 Gen2 Packet Switch<br>
02:01.0 PCI bridge: ASMedia Technology Inc. ASM1184e 4-Port PCIe x1 Gen2 Packet Switch<br>
02:03.0 PCI bridge: ASMedia Technology Inc. ASM1184e 4-Port PCIe x1 Gen2 Packet Switch<br>
02:05.0 PCI bridge: ASMedia Technology Inc. ASM1184e 4-Port PCIe x1 Gen2 Packet Switch<br>
02:07.0 PCI bridge: ASMedia Technology Inc. ASM1184e 4-Port PCIe x1 Gen2 Packet Switch<br>
03:00.0 Network controller: MEDIATEK Corp. MT7921 802.11ax PCI Express Wireless Network Adapter<br>
04:00.0 Non-Volatile memory controller: KIOXIA Corporation NVMe SSD (rev 01)<br>
05:00.0 Ethernet controller: Intel Corporation Ethernet Controller I226-V (rev 04)<br>
<br>
$ ethtool eth1<br>
Settings for eth1:<br>
        Supported ports: [ TP ]<br>
        Supported link modes:   10baseT/Half 10baseT/Full<br>
                                100baseT/Half 100baseT/Full<br>
                                1000baseT/Full<br>
                                2500baseT/Full<br>
        Supported pause frame use: Symmetric<br>
        Supports auto-negotiation: Yes<br>
        Supported FEC modes: Not reported<br>
        Advertised link modes:  10baseT/Half 10baseT/Full<br>
                                100baseT/Half 100baseT/Full<br>
                                1000baseT/Full<br>
                                2500baseT/Full<br>
        Advertised pause frame use: Symmetric<br>
        Advertised auto-negotiation: Yes<br>
        Advertised FEC modes: Not reported<br>
        Speed: 2500Mb/s<br>
        Duplex: Full<br>
        Auto-negotiation: on<br>
        Port: Twisted Pair<br>
        PHYAD: 0<br>
        Transceiver: internal<br>
        MDI-X: off (auto)<br>
netlink error: Operation not permitted<br>
        Current message level: 0x00000007 (7)<br>
                               drv probe link<br>
        Link detected: yes<br>
<br>
<br>
$ nmcli d s<br>
DEVICE         TYPE      STATE                   CONNECTION<br>
wlan0          wifi      connected               WIFI_AP<br>
eth1           ethernet  connected               Wired connection 2<br>
lo             loopback  connected (externally)  lo<br>
p2p-dev-wlan0  wifi-p2p  disconnected            --<br>
eth0           ethernet  unavailable             --<br>
<br>
%%%<br>
Network controller: MEDIATEK Corp. MT7921 802.11ax PCI Express Wireless Network Adapter<br>
のためには　　/boot/firmware/config.txt　　に下記の変更が必要です。<br>
[all]<br>
dtparam=pciex1<br>
dtoverlay=pcie-32bit-dma<br>
