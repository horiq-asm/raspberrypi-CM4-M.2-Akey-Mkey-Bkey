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
04:00.0 Non-Volatile memory controller: Realtek Semiconductor Co., Ltd. RTS5763DL NVMe SSD Controller (DRAM-less) (rev 01)<br>
05:00.0 Ethernet controller: Intel Corporation Ethernet Controller I226-V (rev 04)<br>
<br>
$ nmcli d s<br>
DEVICE         TYPE      STATE                   CONNECTION<br>
wlan0          wifi      connected               WIFI_AP<br>
eth0           ethernet  connected               Wired connection 1<br>
lo             loopback  connected (externally)  lo<br>
p2p-dev-wlan0  wifi-p2p  disconnected            --<br>
eth1           ethernet  unavailable             --<br>
<br>
%%%<br>
Network controller: MEDIATEK Corp. MT7921 802.11ax PCI Express Wireless Network Adapter<br>
のためには　　/boot/firmware/config.txt　　に下記の変更が必要です。<br>
[all]<br>
dtparam=pciex1<br>
dtoverlay=pcie-32bit-dma<br>
