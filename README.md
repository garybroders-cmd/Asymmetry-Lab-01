# Asymmetry-Lab-01

Configure Linux devices</br>
Linux1</br>

vi /etc/networks/interfaces</br>

auto lo</br>
iface lo inet loopback</br>

auto eth0</br>
iface eth0 inet static</br>
	address 192.168.2.100</br>
	netmask 255.255.255.0</br>
	gateway 192.168.2.1</br>

For Linux8</br>

Vi /etc/networks/interfaces</br>

auto lo</br>
iface lo inet loopback</br>

auto eth0</br>
iface eth0 inet static</br>
	address 192.168.8.100</br>
	netmask 255.255.255.0</br>
	gateway 192.168.2.1</br>

Use !wq to save the file.</br>

Restart the network

service hostname restart</br>
