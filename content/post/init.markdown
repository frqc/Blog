# Raspberry Init and Devlopment 

### 烧入镜像
[HypriotsOS](https://github.com/hypriot/image-builder-rpi/releases)

[win32 Disk Imager](https://sourceforge.net/projects/win32diskimager/)

[device-init.yaml](https://github.com/hypriot/device-init)

[config] 中是树莓派硬件相关的设置

hdmi_force_hotplug=1
enable_uart=1

\#camera settings, see http://elinux.org/RPiconfig#Camera
start_x=1
disable_camera_led=1
gpu_mem=128

\# Enable audio (added by raspberrypi-sys-mods)
dtparam=audio=on

# wifi setup
if you are using offical rpi image, create /boot/wpa_supplicant.conf and it will be copied into /etc/wpa_supplicant/wpa_supplicant.conf when booting.

skeleton wpa_supplicant:
```sh
network={
    ssid="YOUR_SSID"
    psk="YOUR_PASSWORD"
    key_mgmt=WPA-PSK
}
```
[ref](https://raspberrypi.stackexchange.com/questions/10251/prepare-sd-card-for-wifi-on-headless-pi?answertab=oldest#tab-top)

also, by default ssh is disable. you need to create an empty ssh file in /boot/ to enable it in booting. 


# ssh
ssh-copy-id user@hostname
sudo chown -v $USER ~/.ssh/known_hosts

vi ~/.ssh/config
```bash
HOST hostname rp1
HostName 192.168.1.114
User pirate
Port 22
```
并修改权限
chmod 600 ~/.ssh/config




# update & install
```bash
sudo apt-get update
sudo apt-get upgrade


```
# python 

install python3
```bash 
sudo apt-get install python3 python3-dev python3-pip
```
repair pip2 and install pip3
```bash 
sudo apt-get purge python-pip
wget https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
sudo python3 get-pip.py
rm get-pip.py
```
```bash
hash -r
```
# golang
```bash
wget https://storage.googleapis.com/golang/go1.8.linux-armv6l.tar.gz
sudo tar -C /usr/local -xzf go1.8.linux-armv6l.tar.gz
```
如果用的是 Hypriot，在 /home/pirate/.profile 中其实已经添加了go 的路径
```sh
# set PATH so it includes GO bin if it exists
if [ -d "/usr/local/go/bin" ] ; then
  PATH="/usr/local/go/bin:$PATH"
  export PATH
fi
```
但需要重新登入（也就是断开ssh 重新连接）才会触发.profle. 若是其他的发行版，将下面一句加入到 ~/.profile 即可。
export PATH=/usr/local/go/bin:$PATH

# node.js
```bash
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
```
# tmux 
```bash
sudo apt-get install -y tmux
```
java?

# shadowsocks
```bash
sudo apt-get install -y shadowsocks
```

```sh
{
    "server":"45.32.61.1",   
    "server_port":9106, 
    "local_address": "0.0.0.0", 
    "local_port":1080,
    "password":"zyc",
    "timeout":600,
    "method":"aes-256-cfb"
}
```

```sh
nohup sslocal -c ~/.sslocal.json >/dev/null 2>&1 
```


~/.proxychains/proxychains.conf
strict_chain
proxy_dns 
remote_dns_subnet 224
tcp_read_time_out 15000
tcp_connect_time_out 8000
localnet 127.0.0.0/255.0.0.0
quiet_mode

[ProxyList]
socks5  127.0.0.1 1080

sslocal -s 45.32.61.1 -p 881  -l 1080 -k kindlefire -t 600 -m aes-256-cfb

## proxychains4
Install 
```bash
git clone https://github.com/rofl0r/proxychains-ng.git
cd proxychains-ng
./configure
(sudo) make && make install
cp ./src/proxychains.conf /etc/proxychians.conf
cd .. && rm -rf proxychains-ng
```
Modify configuration
```
sudo vi /etc/proxychains.conf

strict_chain
proxy_dns 
remote_dns_subnet 224
tcp_read_time_out 15000
tcp_connect_time_out 8000
localnet 127.0.0.0/255.0.0.0
quiet_mode

[ProxyList]
socks5  127.0.0.1 1080
```

proxychains4 
proxychains4 curl https://api.ipify.org/?format=json

Modify /boot/device-init.yaml
nohup sslocal -c ~/sslocal.json >/dev/null 2>&1

sudo vi /etc/rc.local

\# should use absolute path



    `

# Kubernetes






