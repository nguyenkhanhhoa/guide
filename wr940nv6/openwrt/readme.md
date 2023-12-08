# Build from source use imagebuilder on Debian 12
Latest version 19.07.10
# Reference
https://openwrt.org/toh/tp-link/tl-wr940n_v6\
https://openwrt.org/toh/hwdata/tp-link/tp-link_tl-wr940n_v6\
https://archive.openwrt.org/releases/19.07.10/targets/ar71xx/tiny/openwrt-imagebuilder-19.07.10-ar71xx-tiny.Linux-x86_64.tar.xz\
https://openwrt.org/docs/guide-user/additional-software/saving_space

### Install required

sudo apt install build-essential clang flex bison g++ gawk gcc-multilib g++-multilib gettext git libncurses-dev libssl-dev rsync unzip zlib1g-dev file wget

### Build

make -s -j$(nproc ) image PROFILE=tl-wr940n-v6 PACKAGES="uhttpd uhttpd-mod-ubus libiwinfo-lua luci-base luci-app-firewall luci-mod-admin-full luci-theme-bootstrap -ppp -ppp-mod-pppoe -ip6tables -odhcp6c -odhcpd-ipv6only"

## Install:

rename  openwrt-19.07.10-ar71xx-tiny-tl-wr940n-v6-squashfs-factory-eu.bin/openwrt-18.06.9-ar71xx-tiny-tl-wr940n-v6-squashfs-factory-eu.bin to US Version Stock Firmware Upgrade.bin


### Trick from dd wrt to update v6 latest firmware

input to wireless name

`echo "httpd -k"> /tmp/s`

`echo "sleep 10">> /tmp/s`

`echo "httpd -r&">> /tmp/s`

`echo "sleep 10">> /tmp/s`

`echo "httpd -k">> /tmp/s`

`echo "sleep 10">> /tmp/s`

`echo "httpd -f">> /tmp/s`

`sh /tmp/s`

### Recovery 

dd if=wr940nv6.bin of=tplink_boot.bin skip=257 bs=512\
mtd -r write tplink_boot.bin firmware