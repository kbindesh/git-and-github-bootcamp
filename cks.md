## CIS-CAT Lite on Ubuntu 20.04 - Commands

```
vagrant init ubuntu/focal64 

vagrant global-status

vagrant ssh <vm_name_or_id>

sudo apt update
dpkg --list | grep -i jdk - (OPTIONAL)
sudo apt install openjdk-8-jdk -y
sudo apt purge openjdk-8-*  - (OPTIONAL)

java -version

# Download CIS-CAT Lite- https://learn.cisecurity.org/cis-cat-lite
sudo apt update
sudo apt install unzip

sudo su -
unzip /vagrant/Assessor/CIS-CAT Lite Assessor v4.58.0.zip
mv /vagrant/Assessor/ /root/

apt install -y apache2
systemctl status apache2

# Benchmarks/Data-Stream Collections: CIS Ubuntu Linux 20.04 LTS Benchmark v3.0.0
# Profile : Level 1 - Server
sh /root/Assessor/Assessor-CLI.sh -i -rd /var/www/html/ -nts -rp index

(OPTIONAL) cp /var/www/html/index.html /vagrant/index.html
```
