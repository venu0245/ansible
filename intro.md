### INTRODUCTON FOR ANSIBLE-CONFIGURATION:

* Add adapter in vm-box and select `NAT` network interface
* configure yum repostories in a machine
* connection up for the adapter 
 ```
 nmcli con up enp0s8
 nmcli device connected enp0s8
 ```
* install ansible -y
* confgure the epel repository
* search epel-repository
* Refer here[https://docs.fedoraproject.org/en-US/epel/getting-started/] 

