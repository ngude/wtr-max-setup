## setup command sequence

# install qbittorrent lxc
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/ct/qbittorrent.sh)"

# generate and download wireguard config from protonvpn
linux/no filter/no moderate nat/enable nat-pmp and vpn accelerator

# install wireguard and deps on qbittorrent lxc
apt install -y wireguard resolvconf natpmpc curl jq git vim

# specify wireguard config (previously generated, paste into wg0.conf)
vim /etc/wireguard/wg0.conf

# start wireguard
#check your IP before starting so we can compare public IPs
curl ifconfig.me
wg-quick up wg0
systemctl enable wg-quick@wg0

# test to make sure wg is up
# check wireguard connection
wg show
# check the public IP again, it should differ
curl ifconfig.me
# check if port forwarding is enabled and what port we have
natpmpc -g 10.2.0.1 -a 1 0 tcp 60

# run port update script
https://github.com/Simon-CR/qbittorrent-wireguard-pmp

# refer to this link for qbittorrent config info
https://blog.thetechcorner.sk/posts/Qbitorrent-LXC-with-VPN-and-port-forwarding-enabled-in-proxmox-homelab-2.0/#change-network-interface-to-wireguard-vpn
tl;dr - change advanced -> network interface -> wg0; uncheck upnp/nat-pmp; bypass auth for webui on localhost
