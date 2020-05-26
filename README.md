# 小米路由器实现Kcptun加速
### 一、准备工作：
#### 1.经过root的小米路由器（如何root请自行Google）
#### 2.机场-以Google cloud为例
### 二、机场中的SSR和Kcptun的安装
#### 1.googlecloud  SSH进去，获取权限  sudo -i
#### 2.安装ssr 
>wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh

>chmod +x shadowsocks-all.sh

>./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log

选择安装SSR

#### 3.根据自己需求配置好SSR的参数（例如得到如下信息：
| Your Server IP     : |   45.189.161.220 | 
| ---------- | :-----------:  | 
| Your Server Port      : |  50000 | 
| Your Password        : |  123456|
| Your Protocol        :  | origin | 
| Your obfs            : |  plain | 
| Your Encryption Method: |  aes-256-cfb | 

#### 4.安装kcptun

>wget --no-check-certificate https://github.com/kuoruan/shell-scripts/raw/master/kcptun/kcptun.sh

>chmod +x ./kcptun.sh

>./kcptun.sh

#### 5.启动kcptun进行参数配置，注意①keptun客户端连接端口可以选择默认的29900；②主机名称、IPv4地址（加速地址）选择默认的127.0.0.1；③加速的端口需要填写之前SSR配置的端口号，如事例的50000；④kcptun密码可以设置为123456；⑤加密方式选择none；⑥选择fast模式；⑦sndwnd选择512，rcvwnd选择1024；⑧是否关闭数据压缩选择 y；其他都可以用默认设置。最后得出事例结果：

| 服务器IP: |  45.189.161.220  | 
| ---------- | :-----------:  | 
| 端口: |  29900 | 
| 加速地址:  | 127.0.0.1:50000 | 
| key: |  123456 | 
| crypt: |  none | 
| mode:  | fast | 
| mtu: |  1350 | 
| sndwnd:  | 512 | 
| rcvwnd: |  1024 | 
| datashard: |  10 | 
| parityshard: |  3 | 
| dscp: |  0 | 
| nocomp: |  true | 
| quiet: |  false | 
| tcp:  | false | 

以上，在Google cloud的SSR和Kcptun客户端均已配置完成。

### 三、小米路由器的配置安装
#### 1.安装mixbox 

>ssh root@192.168.31.1 #192.168.31.1 替换为自己的路由器网关地址

>sh -c “ $（ curl -kfsSl https://raw.githubusercontent.com/monlor/mbfiles/master/install_github.sh ） ” && 源码 / etc / profile ＆> / dev / null  #安装mixbox客户端

#### 2.安装mixbox内自带插件

#### 3.按照提示配置ssr和kcptun，注意在配置kcptun时：①请输入加速kcp服务器地址中，填写ssr地址，如事例中的 45.189.161.220；②请输入加速kcp服务器端，填写29900；③密码填写123456；④加密方式选择或填写none；⑤检查一下不必要的设置中的一些是否与之前kcptun的设置一致，需要注意的是在请输入加速kcp关闭数据压缩(--nocomp)选择或填写true，其他默认操作即可

### 上述操作完成后，即可实现小米路由器SSR的kcptun加速效果。







