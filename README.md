# ansible-task1-practice
# Task - Create static web site :
  Using ansible run below task on a remote machine 
  install Apache
  download a template from  https://www.free-css.com/free-css-template  to  /var/ www/html 
  Run playbook and test ,make sure web site is accessible over public IP 
  Create a new GitHub Repository
  Once the playbook is ready ,push it to the GitHub Repository

1-craete new repository on github
2-clone the repository into the ansible vm machine.: git clone ssh 

#  manual solution  in VM

1 - Create vm in digital ocean and than run step by step 

2- yum install httpd  -y 

3 - systemctl  start httpd

4-systemctl  enable httpd

5-curl -o  or ( wget )bitcypo.zip  https://www.free-css.com/assets/files/free-css-templates/download/page285/bitcypo.zip

6-yum install unzip -y

7-unzip bitcypo.zip

8-mv   bitcypo-html    /var/www/html

9-cd    /var/www/html/

10-mv   bitcypo-html/* .

11-chown -R  apache:apache    /var/www/html

12-getenforce

13-setenforce 0 

14-getenforce 

15-if you wanna disabled perminently go to this file  ( vi  /etc/sysconfig/selinux )  make it  Selinux= disabled 

16- cat   /etc/sysconfig/selinux

Go to vm and copy ip Number and put it on browser ..you will see  template..
 
That’s it.


#  Playbook solution  in  vsc

Playbook.yml
Tasks:

-google (ansible disable selinux)
-yum
-systemd
-Google (download files with ansible )
-yum
-Google (unzip ansible module)
-Google (ansible charge file ownership)


Run Command :ansible-playbook   playbook.yml

- hosts: vm1
  tasks:
  - name: Disable SElinux
    selinux:
      state: disabled

  - name: Install  Apache
    yum:
      name: httpd
      state: latest    

  - name: Make sure a service  is running
    systemd:
      state: started
      enabled: yes
      name: httpd

  - name: Download template
    get_url:
      url: https://www.free-css.com/assets/files/free-css-templates/download/page285/bitcypo.zip
      dest: /tmp/bitcypo.zip

  - name: Install  unzip
    yum:
      name: unzip
      state: latest

  - name: Extract a template
    unarchive:
      src: /tmp/bitcypo.zip
      dest: /tmp/
      remote_src: yes

  - name: Move templates files to  /var/www/html
    shell: "mv  /tmp/bitcypo-html/*  /var/www/html/"
    args:
      creates: /var/www/html/index.html

  - name: Give insecure permissions to an existing file
    file:
      path: /var/www/html
      owner: apache
      group: apache
      recurse: yes 


cat   /etc/ansible/hosts     = it shows  default inventory file

ansible-playbook   -i  hosts     filename  =    it shows    Custom inventory file

uname -r = it shows current Version

ansible  -i   hosts  all  -m   ping  = it tests to connection




