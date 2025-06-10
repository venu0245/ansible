### INTRODUCTON FOR ANSIBLE-CONFIGURATION:

* Add one adapter in vm-box and select `NAT` 

 ```
 .nmcli con show
 .nmcli dev status
 .nmcli con up enp0s3
 .nmcli dev connect enp0s8
 .netstat -nr
 ```
* set before configuring yum repository
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
 ```
* confgure the epel repository
* search epel-repository
* Refer here[https://docs.fedoraproject.org/en-US/epel/getting-started/] 

