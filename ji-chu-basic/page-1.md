# ✔️ 命令行 - Commands

## Linux

```sh
// 连接SSH服务
ssh 用户名@目标IP
// 批量创建文件
touch 自定义文件名{1..10}
// 指定一个自定义时间去创建文件
touch -d tomorrow 自定义文件名.txt
// 粉碎并删除某个指定文件(无法恢复)
shred -u -z -n 10 文件名.txt
// 复制某文件到指定路径
cp abcd.txt ./tmp/abcd.txt
// 移动某文件到指定路径
mv abcd.txt ./tmp/abcd.txt
// 删除文件夹
rmdir 目录名/
// 创建一个链接，当访问ABCD.txt时，其实会打开abcd.txt
ln -s abcd.txt ABCD.txt
// 添加创建一个新用户账户
useradd 用户名
sudo useradd 用户名
adduser 用户名
sudo adduser 用户名
// 修改指定用户的密码
sudo passwd 用户名
// 查看指定某用户的登录信息、登录状态...（需要先安装:apt install finger）
finger 用户名
// 查询命令、函数、系统调用...
whatis finger
which finger
whereis finger
find / -name "finger"
// 查找所有隐藏文件
sudo find . -type f -name ".*"
// 查找所有空目录
find . -type f -empty
// 查找所有可执行文件
find . -perm /a=x
// 下载互联网上的内容
wget https://具体下载链接
// 将要下载的内容重定向到指定的文件中
curl https://具体下载链接 > abcd.txt
// 创建一个压缩文件(自定义压缩包名和指定待压缩的文件)
zip 压缩包名.zip abcd.txt
// 解压指定压缩包
unzip 压缩包.zip
// 查看指定某文件的开头/结尾
head abcd.txt
tail abcd.txt
// 比较两个指定文件之间的不同
cmp 文件1.txt 文件2.txt
diff 文件1.txt 文件2.txt
// 按照字母顺序对指定内容进行排序
cat abcd.txt | sort 
// 让指定文件可执行
chmod +x abcd.sh
// 更改指定文件的所有权（使指定用户拥有对指定文件的完全控制权）
chown 用户名 abcd.sh
// 过滤（grep/awk/）
ip address | grep eth0
ip address | grep eth0 | grep inet | awk '{print $2}'
// 查找目标系统的DNS解析配置状态
cat /etc/resolv.conf
resolvectl status
// ping一下目标，指定发送自定义个数的数据包和数据包大小（字节）
ping -c 5 -s 600 目标域名地址.com
// 跟踪目标路由路线
traceroute 目标域名地址.com
//显示目标网络统计信息和连接状态的命令
netstat -tulpn
ss -tulpn
// 查看目标系统防火墙情况
sudo iptables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT
// 用于配置和管理 Linux 系统上的防火墙规则
sudo ufw allow 80
sudo ufw enable
sudo ufw status
// 查看当前本机信息（使用neofetch需要先安装：apt install neofetch）
uname -a
neofetch
// 在终端中进行一些运算
echo "33+66+99" | bc
// 查看当前本机上的剩余内存
free
// 查看当前本机上的磁盘剩余情况
df -H
// 显示当前系统上所有进程的详细信息
ps -aux
// 监视当前系统的进程和资源使用情况
top
htop
// 杀掉指定的某个进程（pkill不需要知道进程号）
kill -9 进程号
pkill -f 脚本名
// 开启、停止、关闭指定的某服务
systemctl stop 服务名
systemctl start 服务名
systemctl restart 服务名
systemctl status 服务名
// 其他
history
sudo reboot
sudo shutdown -h now
```





## Windows



