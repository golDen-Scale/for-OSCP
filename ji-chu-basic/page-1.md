# ✔️ 命令行 - Commands

## Linux

<pre class="language-bash"><code class="lang-bash">// 连接SSH服务
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
<strong>cat abcd.txt | sort 
</strong><strong>// 让指定文件可执行
</strong><strong>chmod +x abcd.sh
</strong><strong>// 更改指定文件的所有权（使指定用户拥有对指定文件的完全控制权）
</strong><strong>chown 用户名 abcd.sh
</strong><strong>// 过滤（grep/awk/）
</strong><strong>ip address | grep eth0
</strong><strong>ip address | grep eth0 | grep inet | awk '{print $2}'
</strong><strong>// 查找目标系统的DNS解析配置状态
</strong><strong>cat /etc/resolv.conf
</strong><strong>resolvectl status
</strong><strong>// ping一下目标，指定发送自定义个数的数据包和数据包大小（字节）
</strong><strong>ping -c 5 -s 600 目标域名地址.com
</strong><strong>// 跟踪目标路由路线
</strong><strong>traceroute 目标域名地址.com
</strong><strong>// 显示目标网络统计信息和连接状态的命令
</strong><strong>netstat -tulpn
</strong><strong>ss -tulpn
</strong><strong>// 查看目标系统防火墙情况
</strong><strong>sudo iptables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT
</strong><strong>sudo ufw allow 80
</strong>sudo ufw enable
<strong>sudo ufw status
</strong><strong>// 查看当前本机信息（使用neofetch需要先安装：apt install neofetch）
</strong><strong>uname -a
</strong><strong>neofetch
</strong><strong>// 在终端中进行一些运算
</strong><strong>echo "33+66+99" | bc
</strong><strong>// 查看当前本机上的剩余内存
</strong><strong>free
</strong><strong>df -H
</strong><strong>// 
</strong><strong>ps -aux
</strong></code></pre>





## Windows



