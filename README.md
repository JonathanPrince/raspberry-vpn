# raspberry-vpn

## Set up openvpn

sudo apt-get install openvpn

cd /etc/openvpn

sudo wget https://www.privateinternetaccess.com/openvpn/openvpn.zip

sudo unzip openvpn.zip

sudo rename "s/.ovpn/.conf/" *.ovpn

sudo nano credentials.txt

sudo sed -i 's@auth-user-pass@auth-user-pass /etc/openvpn/credentials.txt@g' *.conf

sudo nano /etc/default/openvpn

AUTOSTART="{openvpnConf}"

sudo iptables -A INPUT -i tun0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A INPUT -i tun0 -j DROP

sudo sh -c "iptables-save > /etc/iptables.nat.vpn.secure"

sudo nano /etc/network/interfaces
up iptables-restore < /etc/iptables.nat.vpn.secure


