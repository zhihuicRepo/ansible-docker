简单的一个通过ansible批量安装docker以及配置flannel网络的playbook(目前仅适用于centos7的Docker初始化)

> 在执行批量安装之前，首先配置在console上安装ansible，具体安装过程参考[ansible安装](https://kevinguo.me/2017/07/06/ansible-client/)


这里并没有配置添加docker repo的过程，如果后续需要安装新版的docker，可以另外在添加

如果有多台主机，在inventories/dev/hosts里面添加hostname即可

实际执行命令如下:

1.ldap登录172.30.33.183,
   cd ~ ,
   git clone http://git.quarkfinance.com/OPS/ansible-docker.git

   $ ssh-keygen -t rsa -P ''
   
   
2.ansible-client上的id_rsa.pub复制到其他所有节点（例如172.30.33.89）
   
   ssh-copy-id -i ~/.ssh/id_rsa.pub 172.30.33.89
   
   输入密码，修改inventories/dev/hosts的ip项为172.30.33.89

3.执行ansible初始化命令
```bash
ansible-playbook -i inventories/dev/hosts deploy-docker.yml
```
4.发生错误排查以下几点 ：  

  a.容器安装路径和安装版本修改roles/docker/defaults/main.yml 中配置dockerd_option_graph: /opt/docker   docker_version: 1.12.6-61.git85d7426.el7.centos  ；  
  b.容器flannel网络yum版本修改roles/flannel/defaults/main.yml中配置 flanneld_version: 0.7.1-2.el7 ；   
  c.初始化容器所需rpm包请上传http://yum.quark.com/docker/centos/7/Packages/ ；  
  d.容器初始化失败时请将repo换成内部源（具体可参考10.19.64.1 /etc/yum.repos.d   ）http://yum.quark.com  ；  
