# Tuning ubuntu
Insufficient RAM can exhaust memory, making it hard to access the server. 
Adding a swap to fast disks (SSD/NVMe) can act as an emergency option if swappiness is minimized.
```bash
# create swap
sudo fallocate -l 2G /swapfile
# set permissions
sudo chmod 600 /swapfile
# mark as swap
sudo mkswap /swapfile
# enabled swap
sudo swapon /swapfile
# verify that the swap is available
sudo swapon --show

# backup fstab
sudo cp /etc/fstab /etc/fstab.bak
# add swap to fstab
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# check swappiness
cat /proc/sys/vm/swappiness
# if swappiness more than 10, then change it to 10
sudo sysctl vm.swappiness=10
# add to /etc/sysctl.conf for permanent change
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
```

# Links
- empty

