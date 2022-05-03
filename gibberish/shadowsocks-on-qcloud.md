---
description: Go back into The Great Firewall to use Netease Music and dear Bilibili!
---

# Shadowsocks on Qcloud

#### Server

1. Buy a student plan for only 10 yuan/month and choose the Operating System as CentOS 7.4. The student plan is at [https://cloud.tencent.com/act/campus](https://cloud.tencent.com/act/campus)\

2.  Get a in-site notification about the basic information of the cloud server just bought including the IP in both internal network and public network. There will also be a default password. Use that to login on the server. Qcloud provides a convenient browser-based terminal which also implements TCP protocol at port 22 (just like SSH). &#x20;

    <img src="broken-reference" alt="" data-size="original">
3.  Now let's begin to install dependency and shadowsocks on the server.

    1.Dependency `# yum install wget curl curl-devel zlib-devel openssl-devel perl perl-devel cpio expat-devel gettext-devel git autoconf libtool gcc swig python-devel`

    2.Download setuptools \
    `# cd /usr/local/src/`\
    `# wget --no-check-certificate  https://pypi.python.org/packages/source/s/setuptools/setuptools-19.6.tar.gz#md5=c607dd118eae682c44ed146367a17e26`\
    `# tar -zvxf setuptools-19.6.tar.gz`\
    `# cd ./setuptools-19.6/`\
    `# python2.7 setup.py build`\
    `# python2.7 setup.py install`

    3.Install pip\
    &#x20; `# yum -y install epel-release` \
    `# yum -y install pip python-pip`\
    `# pip install –upgrade pip`\
    4.Install Shadowsocks\
    &#x20;`# pip install shadowsocks`\
    5.Install encryption package\
    &#x20;`# pip install M2Crypto`
4. Create a configuration file for Shadowsocks server\
   `# cd /etc/`\
   `# nano ./shadowsocks.json                    //create a json file`\
   `======================================================================================`\
   `{`\
   `"server": "(your IP in internal network)",   //e.g. "172.16.0.7" if it doesn't work, just input "0.0.0.0"`\
   `"server_port": 8388,`\
   `"password": "(Create your password)",        //e.g. "12345"`\
   `"timeout": 30,`\
   `"method": "aes-256-cfb"                      //encryption for ss server`\
   `}`\
   `======================================================================================`\
   `(or you can choose a multi-port config for multiple user to connect)`\
   `{`\
   `"server": "(your IP in internal network)",`   \
   `"port_password": {`\
   `"8386": "12345",`\
   `"8387": "23456",`\
   `"8388": "34567"`\
   `},`\
   `"timeout": 30,`\
   `"method": "aes-256-cfb",`\
   `"fast_open": false`\
   `}`\
   `======================================================================================`
5. Create a server and initiate it\
   `# cd /etc/systemd`\
   `# nano ./ssserver.service                     //create a service`\
   `======================================================================================`\
   `[Unit]`\
   `Description=ssserver`\
   `[Service]`\
   `TimeoutStartSec=0`\
   `ExecStart=/usr/bin/ssserver -c /etc/shadowsocks.json`\
   `[Install]`\
   `WantedBy=multi-user.target`\
   `======================================================================================`\
   `# systemctl enable ssserver                   //enable the service`\
   `# systemctl start ssserver`\
   `# systemctl restart ssserver`
6. Check the status \
   `systemctl status ssserver -l` \


We should also config  the Security Group for our cloud server to let it be open to our required protocol ports. \
&#x20;<img src="broken-reference" alt="" data-size="original"> \
&#x20; <img src="broken-reference" alt="" data-size="original"> \
We let the source to be 0.0.0.0/0, which is open to every source. And the TCP:8000-8388 should contain every port you are going to spread to uses(if you are implementing multi-users).\
\
After Client connects to the server, you can use the following to check who's communicating with this port.\
`netstat -anp |grep 'ESTABLISHED' |grep 172.16.0.7:8388         //change the IP to your own Internal Network IP and your port`\


#### Client

Download Shadowsocks on laptop or your cell phone. For iPhone user, Shadowrocket might be an option. Config in Shadowsocks to let it link to your server.\
Use the public IP of your server for "Server address";\
Use the port you assign in json file for "Server port";\
Password and Encryption are both chosen in your json file.\
&#x20;<img src="broken-reference" alt="" data-size="original"> <img src="broken-reference" alt="" data-size="original">&#x20;
