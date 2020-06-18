openssh version: 8.0p1

以3559为例，启动及配置sshd

1. 下载 openssh-arm64 到 3559上，然后解压，进入第一级目录

2. 添加sshd用户，需具有root权限
# cat /etc/passwd 
admin:$1$yi$cOAbMnR6BnMPVujgEQUf01:0:0:root:/:/bin/sh
sshd:$1$yi$cOAbMnR6BnMPVujgEQUf01:0:0:root:/:/bin/sh

3. 添加sshd用户组,组ID为0
# cat /etc/group 
root:x:0:root
sshd:x:0:
nogroup:x:65534:root

4. 优先选级包内库及二进制执行文件
cur=`pwd`
export LD_LIBRARY_PATH=$cur/lib:$LD_LIBRARY_PATH
export PATH=$cur/bin:$cur/sbin:$PATH

5.修改sshd_conf文件中的hostkey路径
HostKey /home/openssh/etc/ssh_host_rsa_key
HostKey /home/openssh/etc/ssh_host_dsa_key
改为
HostKey $cur/etc/ssh_host_rsa_key
HostKey $cur/etc/ssh_host_dsa_key

6. 创建sshd工作目录，这个不能指定，所有使用的编译时候的路径
mkdir -p /home/gjhou/openssh/openssh-8.0p1/__install/

5. 启动ssh server
$cur/sbin/sshd -f $cur/etc/sshd_conf
