# OpenVPN server for Fedora 36

Install OpenVPN and Easy-rsa
# dnf install -y openvpn easy-rsa

Disable SELinux if you want (change SELINUX=disabled) or configure SELinux (hint: semanage port -a -t openvpn_port_t -p "$protocol" "$port")
# nano /etc/selinux/config

Create the easy-rsa folder
# mkdir /etc/openvpn/easy-rsa

Copy the easy-rsa contents to the easy-rsa folder
#cp -air /usr/share/easy-rsa/3/* /etc/openvpn/easy-rsa

Change the directory to the easy-rsa folder
#cd /etc/openvpn/easy-rsa/

Create the certs
# ./easyrsa init-pki
# ./easyrsa build-ca
# ./easyrsa gen-dh
# ./easyrsa build-server-full server nopass
# ./easyrsa build-client-full client nopass
# ./easyrsa gen-crl

Create ta.key
# openvpn --genkey secret /etc/openvpn/easy-rsa/pki/ta.key

Copy the keys and certs in the openvpn server folder
# cd pki $$ cp -rp /etc/openvpn/easy-rsa/pki/ {ca.crt,dh.pem,ta.key,issued,private} /etc/openvpn/server/


Start the OpenVPN Server config script
# curl -O https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh

Make the openvpn-install.sh executable
# chmod +x openvpn-install.sh

Start the OpenVPN Server config script
# ./openvpn-install.sh

Open the port you chose in the script (Default UDP 1194) easyest way is with cockpid
https://yourserverip:9090/network/firewall

Network->Add service->search for openvpn (its default port 1194)
![Screenshot 2022-11-02 224945](https://user-images.githubusercontent.com/18643724/199609243-05316c71-6985-4b22-8dc0-071ddeed68e2.png)
