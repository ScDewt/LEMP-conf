```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install htop vim
```

Append to /etc/ssh/sshd_config 
```bash
CASignatureAlgorithms +ssh-rsa
```

Install certbot and generate ssl certs (detail https://certbot.eff.org/)
```bash
# install snapd
sudo apt install snapd
# ensure that your version of snapd is up to date
sudo snap install core; sudo snap refresh core
# install certbot
sudo snap install --classic certbot
# link certbot to /usr/bin
sudo ln -s /snap/bin/certbot /usr/bin/certbot
# generate ssl certs
sudo certbot --nginx
```