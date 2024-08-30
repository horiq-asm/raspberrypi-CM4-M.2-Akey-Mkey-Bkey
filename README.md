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
<br>
<br>
$ iw phy0 info<br>
Wiphy phy0<br>
        wiphy index: 0<br>
        max # scan SSIDs: 4<br>
        max scan IEs length: 482 bytes<br>
        max # sched scan SSIDs: 10<br>
        max # match sets: 16<br>
        Retry short limit: 7<br>
        Retry long limit: 4<br>
        Coverage class: 0 (up to 0m)<br>
        Device supports AP-side u-APSD.<br>
        Device supports T-DLS.<br>
        Supported Ciphers:<br>
                * WEP40 (00-0f-ac:1)<br>
                * WEP104 (00-0f-ac:5)<br>
                * TKIP (00-0f-ac:2)<br>
                * CCMP-128 (00-0f-ac:4)<br>
                * CCMP-256 (00-0f-ac:10)<br>
                * GCMP-128 (00-0f-ac:8)<br>
                * GCMP-256 (00-0f-ac:9)<br>
                * CMAC (00-0f-ac:6)<br>
                * CMAC-256 (00-0f-ac:13)<br>
                * GMAC-128 (00-0f-ac:11)<br>
                * GMAC-256 (00-0f-ac:12)<br>
        Available Antennas: TX 0x3 RX 0x3<br>
        Configured Antennas: TX 0x3 RX 0x3<br>
        Supported interface modes:<br>
                 * managed<br>
                 * AP<br>
                 * AP/VLAN<br>
                 * monitor<br>
                 * P2P-client<br>
                 * P2P-GO<br>
        Band 1:<br>
                Capabilities: 0x9ff<br>
                        RX LDPC<br>
                        HT20/HT40<br>
                        SM Power Save disabled<br>
                        RX Greenfield<br>
                        RX HT20 SGI<br>
                        RX HT40 SGI<br>
                        TX STBC<br>
                        RX STBC 1-stream<br>
                        Max AMSDU length: 7935 bytes<br>
                        No DSSS/CCK HT40<br>
                Maximum RX AMPDU length 65535 bytes (exponent: 0x003)<br>
                Minimum RX AMPDU time spacing: No restriction (0x00)<br>
                HT TX/RX MCS rate indexes supported: 0-15<br>
                HE Iftypes: managed<br>
                        HE MAC Capabilities (0x08011a000040):<br>
                                +HTC HE Supported<br>
                                Trigger Frame MAC Padding Duration: 2<br>
                                OM Control<br>
                                Maximum A-MPDU Length Exponent: 3<br>
                                A-MSDU in A-MPDU<br>
                        HE PHY Capabilities: (0x2270ce120dc0b306423f00):<br>
                                HE40/2.4GHz<br>
                                242 tone RUs/2.4GHz<br>
                                Device Class: 1<br>
                                LDPC Coding in Payload<br>
                                HE SU PPDU with 1x HE-LTF and 0.8us GI<br>
                                NDP with 4x HE-LTF and 3.2us GI<br>
                                STBC Tx <= 80MHz<br>
                                STBC Rx <= 80MHz<br>
                                Full Bandwidth UL MU-MIMO<br>
                                Partial Bandwidth UL MU-MIMO<br>
                                DCM Max Constellation: 2<br>
                                DCM Max Constellation Rx: 2<br>
                                SU Beamformee<br>
                                Beamformee STS <= 80Mhz: 3<br>
                                Ng = 16 SU Feedback<br>
                                Ng = 16 MU Feedback<br>
                                Codebook Size SU Feedback<br>
                                Codebook Size MU Feedback<br>
                                Triggered CQI Feedback<br>
                                Partial Bandwidth Extended Range<br>
                                PPE Threshold Present<br>
                                Power Boost Factor ar<br>
                                HE SU PPDU & HE PPDU 4x HE-LTF 0.8us GI<br>
                                20MHz in 40MHz HE PPDU 2.4GHz<br>
                                DCM Max BW: 1<br>
                                Longer Than 16HE SIG-B OFDM Symbols<br>
                                Non-Triggered CQI Feedback<br>
                                TX 1024-QAM<br>
                                RX 1024-QAM<br>
                                RX Full BW SU Using HE MU PPDU with Compression SIGB<br>
                                RX Full BW SU Using HE MU PPDU with Non-Compression SIGB<br>
                        HE RX MCS and NSS set <= 80 MHz<br>
                                1 streams: MCS 0-11<br>
                                2 streams: MCS 0-11<br>
                                3 streams: not supported<br>
                                4 streams: not supported<br>
                                5 streams: not supported<br>
                                6 streams: not supported<br>
                                7 streams: not supported<br>
                                8 streams: not supported<br>
                        HE TX MCS and NSS set <= 80 MHz<br>
                                1 streams: MCS 0-11<br>
                                2 streams: MCS 0-11<br>
                                3 streams: not supported<br>
                                4 streams: not supported<br>
                                5 streams: not supported<br>
                                6 streams: not supported<br>
                                7 streams: not supported<br>
                                8 streams: not supported<br>
                        PPE Threshold 0x39 0x1c 0xc7 0x71 0x1c 0x07<br>
                EHT Iftypes: managed<br>
                        EHT MAC Capabilities (0x0000):<br>
                        EHT PHY Capabilities: (0x0000000000000000):<br>
                        EHT MCS/NSS: (0x):<br>
                        EHT bw=20 MHz, max NSS for MCS 0-7: Rx=0, Tx=0<br>
                        EHT bw=20 MHz, max NSS for MCS 8-9: Rx=0, Tx=0<br>
                        EHT bw=20 MHz, max NSS for MCS 10-11: Rx=0, Tx=0<br>
                        EHT bw=20 MHz, max NSS for MCS 12-13: Rx=0, Tx=0<br>
                HE Iftypes: AP<br>
                        HE MAC Capabilities (0x00011a081044):<br>
                                +HTC HE Supported<br>
                                BSR<br>
                                OM Control<br>
                                Maximum A-MPDU Length Exponent: 3<br>
                                BQR<br>
                                A-MSDU in A-MPDU<br>
                                OM Control UL MU Data Disable RX<br>
                        HE PHY Capabilities: (0x0220ce120000a000000c00):<br>
                                HE40/2.4GHz<br>
                                LDPC Coding in Payload<br>
                                NDP with 4x HE-LTF and 3.2us GI<br>
                                STBC Tx <= 80MHz<br>
                                STBC Rx <= 80MHz<br>
                                Full Bandwidth UL MU-MIMO<br>
                                Partial Bandwidth UL MU-MIMO<br>
                                DCM Max Constellation: 2<br>
                                DCM Max Constellation Rx: 2<br>
                                Partial Bandwidth Extended Range<br>
                                PPE Threshold Present<br>
                                TX 1024-QAM<br>
                                RX 1024-QAM<br>
                        HE RX MCS and NSS set <= 80 MHz<br>
                                1 streams: MCS 0-11<br>
                                2 streams: MCS 0-11<br>
                                3 streams: not supported<br>
                                4 streams: not supported<br>
                                5 streams: not supported<br>
                                6 streams: not supported<br>
                                7 streams: not supported<br>
                                8 streams: not supported<br>
                        HE TX MCS and NSS set <= 80 MHz<br>
                                1 streams: MCS 0-11<br>
                                2 streams: MCS 0-11<br>
                                3 streams: not supported<br>
                                4 streams: not supported<br>
                                5 streams: not supported<br>
                                6 streams: not supported<br>
                                7 streams: not supported<br>
                                8 streams: not supported<br>
                        PPE Threshold 0x39 0x1c 0xc7 0x71 0x1c 0x07<br>
                EHT Iftypes: AP<br>
                        EHT MAC Capabilities (0x0000):<br>
                        EHT PHY Capabilities: (0x0000000000000000):<br>
                        EHT MCS/NSS: (0x):<br>
                        EHT bw=20 MHz, max NSS for MCS 0-7: Rx=0, Tx=0<br>
                        EHT bw=20 MHz, max NSS for MCS 8-9: Rx=0, Tx=0<br>
                        EHT bw=20 MHz, max NSS for MCS 10-11: Rx=0, Tx=0<br>
                        EHT bw=20 MHz, max NSS for MCS 12-13: Rx=0, Tx=0<br>
                Bitrates (non-HT):<br>
                        * 1.0 Mbps (short preamble supported)<br>
                        * 2.0 Mbps (short preamble supported)<br>
                        * 5.5 Mbps (short preamble supported)<br>
                        * 11.0 Mbps (short preamble supported)<br>
                        * 6.0 Mbps<br>
                        * 9.0 Mbps<br>
                        * 12.0 Mbps<br>
                        * 18.0 Mbps<br>
                        * 24.0 Mbps<br>
                        * 36.0 Mbps<br>
                        * 48.0 Mbps<br>
                        * 54.0 Mbps<br>
                Frequencies:<br>
                        * 2412 MHz [1] (20.0 dBm)<br>
                        * 2417 MHz [2] (20.0 dBm)<br>
                        * 2422 MHz [3] (20.0 dBm)<br>
                        * 2427 MHz [4] (20.0 dBm)<br>
                        * 2432 MHz [5] (20.0 dBm)<br>
                        * 2437 MHz [6] (20.0 dBm)<br>
                        * 2442 MHz [7] (20.0 dBm)<br>
                        * 2447 MHz [8] (20.0 dBm)<br>
                        * 2452 MHz [9] (20.0 dBm)<br>
                        * 2457 MHz [10] (20.0 dBm)<br>
                        * 2462 MHz [11] (20.0 dBm)<br>
                        * 2467 MHz [12] (20.0 dBm)<br>
                        * 2472 MHz [13] (20.0 dBm)<br>
                        * 2484 MHz [14] (20.0 dBm)<br>
        Band 2:<br>
                Capabilities: 0x9ff<br>
                        RX LDPC<br>
                        HT20/HT40<br>
                        SM Power Save disabled<br>
                        RX Greenfield<br>
                        RX HT20 SGI<br>
                        RX HT40 SGI<br>
                        TX STBC<br>
                        RX STBC 1-stream<br>
                        Max AMSDU length: 7935 bytes<br>
                        No DSSS/CCK HT40<br>
                Maximum RX AMPDU length 65535 bytes (exponent: 0x003)<br>
                Minimum RX AMPDU time spacing: No restriction (0x00)<br>
                HT TX/RX MCS rate indexes supported: 0-15<br>
                VHT Capabilities (0x339071b2):<br>
                        Max MPDU length: 11454<br>
                        Supported Channel Width: neither 160 nor 80+80<br>
                        RX LDPC<br>
                        short GI (80 MHz)<br>
                        TX STBC<br>
                        SU Beamformee<br>
                        MU Beamformee<br>
                        RX antenna pattern consistency<br>
                        TX antenna pattern consistency<br>
                VHT RX MCS set:<br>
                        1 streams: MCS 0-9<br>
                        2 streams: MCS 0-9<br>
                        3 streams: not supported<br>
                        4 streams: not supported<br>
                        5 streams: not supported<br>
                        6 streams: not supported<br>
                        7 streams: not supported<br>
                        8 streams: not supported<br>
                VHT RX highest supported: 0 Mbps<br>
                VHT TX MCS set:<br>
                        1 streams: MCS 0-9<br>
                        2 streams: MCS 0-9<br>
                        3 streams: not supported<br>
                        4 streams: not supported<br>
                        5 streams: not supported<br>
                        6 streams: not supported<br>
                        7 streams: not supported<br>
                        8 streams: not supported<br>
                VHT TX highest supported: 0 Mbps<br>
                VHT extended NSS: supported<br>
                HE Iftypes: managed<br>
                        HE MAC Capabilities (0x08011a000040):<br>
                                +HTC HE Supported<br>
                                Trigger Frame MAC Padding Duration: 2<br>
                                OM Control<br>
                                Maximum A-MPDU Length Exponent: 3<br>
                                A-MSDU in A-MPDU<br>
                        HE PHY Capabilities: (0x4470ce120dc0b306423f00):<br>
                                HE40/HE80/5GHz<br>
                                242 tone RUs/5GHz<br>
                                Device Class: 1<br>
                                LDPC Coding in Payload<br>
                                HE SU PPDU with 1x HE-LTF and 0.8us GI<br>
                                NDP with 4x HE-LTF and 3.2us GI<br>
                                STBC Tx <= 80MHz<br>
                                STBC Rx <= 80MHz<br>
                                Full Bandwidth UL MU-MIMO<br>
                                Partial Bandwidth UL MU-MIMO<br>
                                DCM Max Constellation: 2<br>
                                DCM Max Constellation Rx: 2<br>
                                SU Beamformee<br>
                                Beamformee STS <= 80Mhz: 3<br>
                                Ng = 16 SU Feedback<br>
                                Ng = 16 MU Feedback<br>
                                Codebook Size SU Feedback<br>
                                Codebook Size MU Feedback<br>
                                Triggered CQI Feedback<br>
                                Partial Bandwidth Extended Range<br>
                                PPE Threshold Present<br>
                                Power Boost Factor ar<br>
                                HE SU PPDU & HE PPDU 4x HE-LTF 0.8us GI<br>
                                20MHz in 40MHz HE PPDU 2.4GHz<br>
                                DCM Max BW: 1<br>
                                Longer Than 16HE SIG-B OFDM Symbols<br>
                                Non-Triggered CQI Feedback<br>
                                TX 1024-QAM<br>
                                RX 1024-QAM<br>
                                RX Full BW SU Using HE MU PPDU with Compression SIGB<br>
                                RX Full BW SU Using HE MU PPDU with Non-Compression SIGB<br>
                        HE RX MCS and NSS set <= 80 MHz<br>
                                1 streams: MCS 0-11<br>
                                2 streams: MCS 0-11<br>
                                3 streams: not supported<br>
                                4 streams: not supported<br>
                                5 streams: not supported<br>
                                6 streams: not supported<br>
                                7 streams: not supported<br>
                                8 streams: not supported<br>
                        HE TX MCS and NSS set <= 80 MHz<br>
                                1 streams: MCS 0-11<br>
                                2 streams: MCS 0-11<br>
                                3 streams: not supported<br>
                                4 streams: not supported<br>
                                5 streams: not supported<br>
                                6 streams: not supported<br>
                                7 streams: not supported<br>
                                8 streams: not supported<br>
                        PPE Threshold 0x39 0x1c 0xc7 0x71 0x1c 0x07<br>
                EHT Iftypes: managed<br>
                        EHT MAC Capabilities (0x0000):<br>
                        EHT PHY Capabilities: (0x0000000000000000):<br>
                        EHT MCS/NSS: (0x):<br>
                        EHT bw <= 80 MHz, max NSS for MCS 8-9: Rx=0, Tx=0<br>
                        EHT bw <= 80 MHz, max NSS for MCS 10-11: Rx=0, Tx=0<br>
                        EHT bw <= 80 MHz, max NSS for MCS 12-13: Rx=0, Tx=0<br>
                HE Iftypes: AP<br>
                        HE MAC Capabilities (0x00011a081044):<br>
                                +HTC HE Supported<br>
                                BSR<br>
                                OM Control<br>
                                Maximum A-MPDU Length Exponent: 3<br>
                                BQR<br>
                                A-MSDU in A-MPDU<br>
                                OM Control UL MU Data Disable RX<br>
                        HE PHY Capabilities: (0x0420ce120000a000000c00):<br>
                                HE40/HE80/5GHz<br>
                                LDPC Coding in Payload<br>
                                NDP with 4x HE-LTF and 3.2us GI<br>
                                STBC Tx <= 80MHz<br>
                                STBC Rx <= 80MHz<br>
                                Full Bandwidth UL MU-MIMO<br>
                                Partial Bandwidth UL MU-MIMO<br>
                                DCM Max Constellation: 2<br>
                                DCM Max Constellation Rx: 2<br>
                                Partial Bandwidth Extended Range<br>
                                PPE Threshold Present<br>
                                TX 1024-QAM<br>
                                RX 1024-QAM<br>
                        HE RX MCS and NSS set <= 80 MHz<br>
                                1 streams: MCS 0-11<br>
                                2 streams: MCS 0-11<br>
                                3 streams: not supported<br>
                                4 streams: not supported<br>
                                5 streams: not supported<br>
                                6 streams: not supported<br>
                                7 streams: not supported<br>
                                8 streams: not supported<br>
                        HE TX MCS and NSS set <= 80 MHz<br>
                                1 streams: MCS 0-11<br>
                                2 streams: MCS 0-11<br>
                                3 streams: not supported<br>
                                4 streams: not supported<br>
                                5 streams: not supported<br>
                                6 streams: not supported<br>
                                7 streams: not supported<br>
                                8 streams: not supported<br>
                        PPE Threshold 0x39 0x1c 0xc7 0x71 0x1c 0x07<br>
                EHT Iftypes: AP<br>
                        EHT MAC Capabilities (0x0000):<br>
                        EHT PHY Capabilities: (0x0000000000000000):<br>
                        EHT MCS/NSS: (0x):<br>
                        EHT bw <= 80 MHz, max NSS for MCS 8-9: Rx=0, Tx=0<br>
                        EHT bw <= 80 MHz, max NSS for MCS 10-11: Rx=0, Tx=0<br>
                        EHT bw <= 80 MHz, max NSS for MCS 12-13: Rx=0, Tx=0<br>
                Bitrates (non-HT):<br>
                        * 6.0 Mbps<br>
                        * 9.0 Mbps<br>
                        * 12.0 Mbps<br>
                        * 18.0 Mbps<br>
                        * 24.0 Mbps<br>
                        * 36.0 Mbps<br>
                        * 48.0 Mbps<br>
                        * 54.0 Mbps<br>
                Frequencies:<br>
                        * 5180 MHz [36] (20.0 dBm)<br>
                        * 5200 MHz [40] (20.0 dBm)<br>
                        * 5220 MHz [44] (20.0 dBm)<br>
                        * 5240 MHz [48] (20.0 dBm)<br>
                        * 5260 MHz [52] (20.0 dBm) (radar detection)<br>
                        * 5280 MHz [56] (20.0 dBm) (radar detection)<br>
                        * 5300 MHz [60] (20.0 dBm) (radar detection)<br>
                        * 5320 MHz [64] (20.0 dBm) (radar detection)<br>
                        * 5500 MHz [100] (23.0 dBm) (radar detection)<br>
                        * 5520 MHz [104] (23.0 dBm) (radar detection)<br>
                        * 5540 MHz [108] (23.0 dBm) (radar detection)<br>
                        * 5560 MHz [112] (23.0 dBm) (radar detection)<br>
                        * 5580 MHz [116] (23.0 dBm) (radar detection)<br>
                        * 5600 MHz [120] (23.0 dBm) (radar detection)<br>
                        * 5620 MHz [124] (23.0 dBm) (radar detection)<br>
                        * 5640 MHz [128] (23.0 dBm) (radar detection)<br>
                        * 5660 MHz [132] (23.0 dBm) (radar detection)<br>
                        * 5680 MHz [136] (23.0 dBm) (radar detection)<br>
                        * 5700 MHz [140] (23.0 dBm) (radar detection)<br>
                        * 5720 MHz [144] (disabled)<br>
                        * 5745 MHz [149] (disabled)<br>
                        * 5765 MHz [153] (disabled)<br>
                        * 5785 MHz [157] (disabled)<br>
                        * 5805 MHz [161] (disabled)<br>
                        * 5825 MHz [165] (disabled)<br>
                        * 5845 MHz [169] (disabled)<br>
                        * 5865 MHz [173] (disabled)<br>
                        * 5885 MHz [177] (disabled)<br>
        Supported commands:<br>
                 * new_interface<br>
                 * set_interface<br>
                 * new_key<br>
                 * start_ap<br>
                 * new_station<br>
                 * new_mpath<br>
                 * set_mesh_config<br>
                 * set_bss<br>
                 * authenticate<br>
                 * associate<br>
                 * deauthenticate<br>
                 * disassociate<br>
                 * join_ibss<br>
                 * join_mesh<br>
                 * remain_on_channel<br>
                 * set_tx_bitrate_mask<br>
                 * frame<br>
                 * frame_wait_cancel<br>
                 * set_wiphy_netns<br>
                 * set_channel<br>
                 * tdls_mgmt<br>
                 * tdls_oper<br>
                 * start_sched_scan<br>
                 * probe_client<br>
                 * set_noack_map<br>
                 * register_beacons<br>
                 * start_p2p_device<br>
                 * set_mcast_rate<br>
                 * connect<br>
                 * disconnect<br>
                 * channel_switch<br>
                 * set_qos_map<br>
                 * set_multicast_to_unicast<br>
                 * set_sar_specs<br>
        WoWLAN support:<br>
                 * wake up on disconnect<br>
                 * wake up on magic packet<br>
                 * wake up on pattern match, up to 1 patterns of 1-128 bytes,<br>
                   maximum packet offset 0 bytes<br>
                 * can do GTK rekeying<br>
                 * wake up on network detection, up to 10 match sets<br>
        software interface modes (can always be added):<br>
                 * AP/VLAN<br>
                 * monitor<br>
        valid interface combinations:<br>
                 * #{ managed, P2P-client } <= 2, #{ AP, P2P-GO } <= 1,<br>
                   total <= 2, #channels <= 2<br>
        HT Capability overrides:<br>
                 * MCS: ff ff ff ff ff ff ff ff ff ff<br>
                 * maximum A-MSDU length<br>
                 * supported channel width<br>
                 * short GI for 40 MHz<br>
                 * max A-MPDU length exponent<br>
                 * min MPDU start spacing<br>
        Device supports TX status socket option.<br>
        Device supports HT-IBSS.<br>
        Device supports SAE with AUTHENTICATE command<br>
        Device supports scan flush.<br>
        Device supports per-vif TX power setting<br>
        Driver supports full state transitions for AP/GO clients<br>
        Driver supports a userspace MPM<br>
        Device supports active monitor (which will ACK incoming frames)<br>
        Driver/device bandwidth changes during BSS lifetime (AP/GO mode)<br>
        Device supports configuring vdev MAC-addr on create.<br>
        Device supports randomizing MAC-addr in scans.<br>
        Device supports randomizing MAC-addr in sched scans.<br>
        max # scan plans: 1<br>
        max scan plan interval: 65535<br>
        max scan plan iterations: 0<br>
        Supported TX frame types:<br>
                 * IBSS: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0<br>
                 * managed: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0<br>
                 * AP: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0<br>
                 * AP/VLAN: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0<br>
                 * mesh point: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0<br>
                 * P2P-client: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0<br>
                 * P2P-GO: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0<br>
                 * P2P-device: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0<br>
        Supported RX frame types:<br>
                 * IBSS: 0x40 0xb0 0xc0 0xd0<br>
                 * managed: 0x40 0xb0 0xd0<br>
                 * AP: 0x00 0x20 0x40 0xa0 0xb0 0xc0 0xd0<br>
                 * AP/VLAN: 0x00 0x20 0x40 0xa0 0xb0 0xc0 0xd0<br>
                 * mesh point: 0xb0 0xc0 0xd0<br>
                 * P2P-client: 0x40 0xd0<br>
                 * P2P-GO: 0x00 0x20 0x40 0xa0 0xb0 0xc0 0xd0<br>
                 * P2P-device: 0x40 0xd0<br>
        Supported extended features:<br>
                * [ RRM ]: RRM<br>
                * [ SET_SCAN_DWELL ]: scan dwell setting<br>
                * [ BEACON_RATE_LEGACY ]: legacy beacon rate setting<br>
                * [ BEACON_RATE_HT ]: HT beacon rate setting<br>
                * [ BEACON_RATE_VHT ]: VHT beacon rate setting<br>
                * [ FILS_STA ]: STA FILS (Fast Initial Link Setup)<br>
                * [ CQM_RSSI_LIST ]: multiple CQM_RSSI_THOLD records<br>
                * [ CONTROL_PORT_OVER_NL80211 ]: control port over nl80211<br>
                * [ ACK_SIGNAL_SUPPORT ]: ack signal level support<br>
                * [ TXQS ]: FQ-CoDel-enabled intermediate TXQs<br>
                * [ CAN_REPLACE_PTK0 ]: can safely replace PTK 0 when rekeying<br>
                * [ AIRTIME_FAIRNESS ]: airtime fairness scheduling<br>
                * [ AQL ]: Airtime Queue Limits (AQL)<br>
                * [ CONTROL_PORT_NO_PREAUTH ]: disable pre-auth over nl80211 control port support<br>
                * [ SCAN_FREQ_KHZ ]: scan on kHz frequency support<br>
                * [ CONTROL_PORT_OVER_NL80211_TX_STATUS ]: tx status for nl80211 control port support<br>
                * [ BEACON_RATE_HE ]: HE beacon rate support (AP/mesh)<br>
