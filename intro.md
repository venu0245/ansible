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
* vim /etc/ansible/ansible.cfg
* Refer here[https://github.com/ansible/ansible/blob/stable-2.9/examples/ansible.cfg]
  
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
  
   Ansible_config             |       Envirnomental Variables
 --------------------------------------------------------------
 1. ansible.cfg               |     .in current working directory
 2. .ansible.cfg              |     .user's home directory
 3. /etc/ansible/ansible.cfg  |     .system wide default directory
 
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
* if client server not available,we get

 * ansible vhost2 -m ping
 ```
 vhost2 | UNREACHABLE! => {
    "changed": false,
    "msg": "Failed to connect to the host via ssh: ssh: connect to host vhost2 port 22: No route to host",
    "unreachable": true
}

 ``` 
#### Ansible Inventories:
* from `/etc/hosts to /etc/ansible/hosts`

 ```
* vim /etc/hosts 
  192.168.10.60  venu60.git.com   vhost1
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
}
 ```   
#### Ansible group related hosts
* vim /etc/ansible/hosts
* vim /etc/hosts
 ```
 * vim /etc/ansible/hosts
  [app]
  vhost1
  vhost2

  [app]
  vhost1
  [web]
  vhost2
 ```
* ansible all -m ping -o
* ansible app -m ping
* ansible web -m ping 

* port based variables 
 ```
 * vim /etc/ansible/hosts

  vhost1 ansible_port=555
  vhost2
 ``` 
* ansible all -m ping -o

* user based variables
 ```
 * vim /etc/ansible/hosts

  vhost1
  vhost2 ansible_user=ansible
 ``` 
* ansible all -m ping -o

#### Ansible group related variables
 ```
 * vim /etc/ansible/hosts
   [app]
  vhost1
  [web]
  vhost2

  [web:vars]
  ansible_port=555
  [app]
  ansible_user=ansible
 ```
* ansible all -m ping -o

#### Ansible modules
* modules are nothing but set of commands or set of task

* modules:
 ```
 ping
 ps
 copy
 services
 ls
 top
 vmstat
 dnf
 ```
#### Ansible ad-hoc commands: 
* run a command on all hosts or specific host or group related hosts or group related variables hosts
 ```
 ansible vhost1 -m command -a "ps"
 ansible app -m command -a "uptime"
 ansible [app:vars] -m command -a "date"
 ansible all -m command -a "tail /etc/passwd"
 ``` 
* create a file and directory
 ```
 ansible all -m file -a "path=web state=touch"
 ansible all -m file -a "path=app state=directory"
 ```
* create a user and delete
 ```
 ansible all -m user -a "name=ram state=present"
 ansible all -a "id ram"
 ansible all -m user -a "name=ram state=absent"
 ```  
* checking status of a service
* sevice in ansible command line
 ```
 ansible all -m service -a "name=vsftpd.service state=status"
 ansible all -m service -a "name=vsftpd.service state=restarted"
 ansible all -m service -a "name=vsftpd.service state=stopped"
 
 ```
#### Ansible Plays and Playbooks:
* ansible play: consists of tasks to perform on client machines
* ansible playbook: set of plays to execute on remote machines
* ansible playbook are in yaml format
* ansible playbook starts with --- at the top
 
* create/delete a file module in ansible playbook
 
 ```
 . ansible all -m file -a "name=web state=touch"

 - hosts: all
   become: yes
   tasks: 
          - name: create a file
            file:
                  path: web
                  state: touch

 . ansible all -m file -a "name=web state=absent"

  - hosts: all
   become: yes
   tasks:
          - name: status of service
            file:
                     name: web
                     state: absent

  . become means gather the information about playbook
  . gather_facts means collect the information about playbook                    
                     
  . ansible-playbook app.yml --syntax-check
  . ansible-playbook app.yml                
 ```  

* create/delete a user module in ansible playbook 
 
 ```
 - hosts: vhost1
   become: yes
   tasks:
          - name: create a user
            user:
                  name: preethi
                  state: present
                  remove: no

  .ansible-playbook app1.yml --syntax-check
  .ansible-playbook app.yml                
              
  .delete a user module in ansible playbook
 
  - hosts: vhost1
   become: yes
   tasks:
          - name: delete a user
            user:
                  name: preethi
                  state: absent
                  remove: yes

  .ansible-playbook app1.yml --syntax-check
  .ansible-playbook app1.yml   

  ```
* service module status,stopped,started in ansible playbook

 ```
  . ansible all -m service -a "name=nfs-server.service state=status"

 - hosts: all
   become: yes
   tasks:
          - name: status of service
            service:
                     name: nsfs-server.service
                     state: status

  . ansible all -m service -a "name=nfs-server.service state=stopped"

 - hosts: all
   become: yes
   tasks:
          - name: status of service
            service:
                     name: nsfs-server.service
                     state: stopped

  . ansible all -m service -a "name=nfs-server.service state=started"
   
 - hosts: all
   become: yes
   tasks:
          - name: status of service
            service:
                     name: nsfs-server.service
                     state: started  

 .ansible-playbook app2.yml --syntax-check
 .ansible-playbook app2.yml                      

 ```

* install and removed a package in ansible playbook 

 ```
  . ansible all -m dnf -a "name=httpd state=latest
 
 - hosts: all
   become: yes
   tasks: 
          - name: install a package
            dnf:
                 name: httpd
                 state: latest
 
  . remove a package in ansible playbook
 
  . ansible all -m dnf -a "name=httpd state=removed
  
 - hosts: all
   become: yes
   tasks:
          - name: remove httpd package
            dnf:
                 name: httpd
                 state: removed

 . install multiple packages
  
  - hosts: all
    become: yes
    tasks:
            - name: installing packages
              dnf:
                   name:
                      - httpd
                      - bind
                      - sysstat
                   state: latest                        

 . ansible-playbook app3.yml --syntax-check
 . ansible-playbook app3.yml

 ``` 
#### Ansible Facts:
* Ansible facts are data gathered about target nodes (host nodes to be configured) and returned back to controller nodes. Ansible facts are stored in JSON format
 ```
 ansible-doc setup
  ```
 
  ```
  .ansible all -m setup

  - hosts: all
    become: yes
    tasks:
            - name: ansible facts
              debug:
                     msg: "{{ ansible_facts }}"

 .checking hostnames
  - hosts: all
    become: yes     
    tasks:
           - name: hostname
             debug:
                    msg: "{{ ansible_facts.hostame }}"

 .checking selinux
  - hosts: all
    become: yes     
    tasks:
           - name: hostname
             debug:
                    msg: "{{ ansible_facts.selinux }}"                   
  ```
* checking all setup facts
 ```
 - hosts: vhost1
  tasks:
          - name: facts
            debug:
                    msg: "{{ ansible_facts.hostname }}"
          - name: facts
            debug:
                    msg: "{{ ansible_facts.domain }}"
          - name: selinux
            debug:
                    msg: "{{ ansible_facts.selinux }}"


          - name: ip address
            debug:
                    msg: "{{ ansible_facts.all_ipv4_addresses }}"

          - name: ethernet
            debug:
                    msg: "{{ ansible_facts.enp0s8 }}"
    ============================================================================
  . ansible-playbook app9.yml --syntax-check
  . ansible-playbook app9.yml

  . Result:
  
     TASK [Gathering Facts] ****************************************************************************************************************
ok: [vhost1]

TASK [facts] **************************************************************************************************************************
ok: [vhost1] => {
    "msg": "venu60"
}

TASK [facts] **************************************************************************************************************************
ok: [vhost1] => {
    "msg": "git.com"
}

TASK [selinux] ************************************************************************************************************************
ok: [vhost1] => {
    "msg": {
        "config_mode": "enforcing",
        "mode": "enforcing",
        "policyvers": 33,
        "status": "enabled",
        "type": "targeted"
    }
}

TASK [ip address] *********************************************************************************************************************
ok: [vhost1] => {
    "msg": [
        "192.168.10.60",
        "10.0.3.15",
        "192.168.122.1"
    ]

 ```



 

  
  



  
 
 


