### INTRODUCTON FOR ANSIBLE-CONFIGURATION:

* Add an one adapter in vm-box and select  `settings->network->adapter2->NAT` 

 ```
 .nmcli con show
 .nmcli dev status
 .nmcli con up enp0s3
 .nmcli dev connect enp0s8
 .netstat -nr
 ```
* configuring yum repository
 ```
 .mount /dev/sr0 /media
 .cd /media/Appstream/Packages
 .rpm -ivh vsftpd.service
 .systemctl enable --now vsftpd.service
 .vim /etc/vsftpd/vsftpd.conf
   anonymous_enable=YES
 .firewall-cmd --list-all  
 .firewall-cmd --add-service=ftp --permanent
 .firewall-cmd --reload
 .cd /var/ftp/pub
 .cp -rvf /media /var/ftp/pub/<name_folder>
 ```
* from server side can be configure

* vim /etc/yum.repos.d/my.repo
 ```
 .[AppStream]
name=Local_AppStream_repo
baseurl=file:///var/ftp/pub/venu/AppStream
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[BaseOS]
name=Local_BaseOS_repo
baseurl=file:///var/ftp/pub/venu/BaseOS
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

 ```
* dnf clean all
* dnf provides ansible

* from client side can be configure

* vim /etc/yum.repos.d/cl.repo
 ```
 .[AppStream]
name=FTP_AppStream_repo
baseurl=ftp://192.168.10.60/pub/venu/AppStream
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[BaseOS]
name=FTP_BaseOS_repo
baseurl=ftp://192.168.10.60/pub/venu/BaseOS
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

 ```  
* installing packages for ansible
 ``` 
  .dnf install ansible* -y
  .dnf list ansible*
  .dnf info ansible*
  .dnf repolist
 ```
* confgure the epel repository
* search epel-repository rhel8 on website
* Refer here[https://docs.fedoraproject.org/en-US/epel/]
* Refer here[https://docs.fedoraproject.org/en-US/epel/getting-started/] go through link and get the url EL8
* ```
  .dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
  .dnf install ansible* -y
  .dnf repolist
  ```
* enter the client ip-address in server for example:192.168.10.60->server and 192.168.10.61->client
* very client machine have hostname
 ```
 .vim /etc/hosts
  venu60.git.com  192.168.10.60
  venu61.git.com  192.168.10.61
  venu62.git.com  192.168.10.62  
 ```
* ansible configuration file in that can be copy the link and browse on web we can get and copy the file 
 ```
 .vim /etc/ansible/ansible.cfg
  https://github.com/ansible/ansible/blob/stable-2.9/examples/ansible.cfg
 ``` 
 * setup passwordless login for `ssh-keygen` from all machines

 #### Ansible Core Components:
  ```
  .Ansible Configuration File
  .Ansible Inventories
  .Ansible Modules
  .Ansible Variables
  .Ansible Facts
  .Ansible Plays
  .Ansible Playbooks
  ```
  
  Ansible_config                    Envirnomental Variables
 --------------------------------------------------------------
 1. ansible.cfg                    .in current working directory
 2. .ansible.cfg                   .user's home directory
 3. /etc/ansible/ansible.cfg       .system wide default directory
 
* ansibile --version 
1. Ansible configuration File:

* default ansible configuration file is
 ```
 vim /etc/ansible/ansible.cfg
 ```  
* create a user and switch to user for ansible purpose
 ```
 useradd ansible
 password ansible
 su - ansible
 ``` 
* user creates a own configuration file
 ```
 .touch /tmp/ansible/ansible.cfg
 .touch ansible.cfg
 .touch .ansible.cfg
 ```
* when user run a command as `ansible --version` from the above any one of them can be genrated,if now of them not create a own confuration file we get default configuration file for ansible
 ```
 vim /etc/ansible/ansible.cfg
 ```
* configure ssh 
 ```
  ssh 192.168.10.60
  ssh-keygen
  ssh-copy-id -i id_rsa.pub 192.168.10.61   60->61
 ```

* vim /etc/hosts
  ```
  192.168.10.60  venu60.git.com  vhost1
  192.168.10.61  venu61.git.com  vhost2
  192.168.10.62  venu62.git.com  vhost3
  ``` 
* vim /etc/ansible/hosts
 ```
 vhost1
 vhost2
 vhost3
 ``` 
* test the ansible ping module
 ```
 ansible vhost1 -m ping
 ansible vhost2 -m ping
 ansible all -m ping
 ansible vhost1 -m ping -o
 ```  
* ansible vhost1 -m ping
 ```
 vhost1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}

 ``` 
#### Ansible Inventories:
* from `/etc/hosts to /etc/ansible/hosts`

 ```
* vim /etc/hosts 
  192.168.10.60  venu60.git.com  vhost1
  192.168.10.61  venu61.git.com   vhost2
  192.168.10.62  venu62.git.com   vhost3
 ```

 ```
 * vim /etc/ansible/hosts
   vhost[1:3]
 ``` 
* ansible all -m ping
 ```
 vhost1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}
vhost2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"

 ```   


  
 
 


