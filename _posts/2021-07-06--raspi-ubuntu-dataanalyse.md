---
layout: post
title: 树莓派打造量化平台
date: 2021-07-06
categories: blog
tags: [ubuntu,树莓派,数字货币,量化,股票]
description: 树莓派打造量化平台
---


# digitalcurrencyanalyse

## **1. 数字货币自动交易策略框架** ##

数字货币自动交易策略框架,通过对接交易所API配合自己的交易策略进行自动化交易

## **2. 软件架构** ##
  技术和工具：Python3.8+钉钉+微信+redis+宝塔

  树莓派服务器：
  
    ubuntu-001：策略交易服务器
    ubnutu-002：策略分析服务器+数据库服务器

## **3. 安装教程** ##

  宝塔控制面板里添加python服务脚本

## **4. 使用说明**
  ### **1. 策略实现**
    1.1 网格交易策略实现-已实现
  ### **2. 交易所API对接**
    2.1抹茶-实现
    2.2币安-完成中
    2.3aex、火币、币安-未实现
  ### **3. 其他提醒功能**
    3.1钉钉提醒
    钉钉交易提醒，系统异常监控提醒

    3.2微信提醒
    微信交易提醒、微信各种命令查询提醒

  ### **4. 功能说明**
  
  #### **4.1数字策略运行监控**

        配置文件：
        config_strategy_grid_reminder.ini
        [configs.stock.client]
        check_client_process={
            'start_check':1,//开始检测标识
            'before_time_zone':['09:20:00','11:30:00'],//检测上午时间段
            'after_time_zone':['12:50:00','15:00:00'],//检测下午时间段
            'process_list':['product_trade_strategy_grid_A.py','product_trade_strategy_grid_B.py'],
            //监控python进程名，即策略的运行名
            'check_span_time_second':300,//检测时间段，300秒
            'check_span_time_second_zone':30,//
            'serviceip':'192.168.10.149:8887',//服务器ip
            'weekdayzone':"0-4"//周一到周五
            }

  #### **4.2数字策略运行监控**

      微信机器人：
      后面需要完善的功能：
      1.访问限制：60分钟20条查询限制
      2.加入更多行情指标
  #### **4.3书籍下载**
      待续......

## **5. 环境清单**
#### **5.0配置说明**
      0.策略轮询配置
        [configs.timer]
        day_of_week:0-6----周一到周六
        hour:0-23----0-23点轮询
        minute:*/1----代表秒，如果为数字则为分钟，如minute:5，代表5分钟轮询一次
      1.提醒配置：
          [configs.stock.client]
          check_client_process={
              'start_check':1,
              'before_time_zone':['09:20:00','11:30:00'],
              'after_time_zone':['12:50:00','15:00:00'],
              'process_list':['product_trade_strategy_grid_A.py','product_trade_strategy_grid_B.py'],
              'check_span_time_second':300,
              'check_span_time_second_zone':30,
              'serviceip':'192.168.10.149:8887',
              'weekdayzone':"0-4"
              }

      2.交易配置说明
          [configs.digitcurrency]
          dc_ltc2l_usdt_mxc008-e-167 = {
              'version':20210114,
              'serviceip':'192.168.10.149:8887',
              'contextid':'dc_ltc2l_usdt_mxc008-e-167',
              'strategy':'DC_Trade_Strategy_Grid_E',
              'trigger_start':False,
              'current_dt': {'month': 9},
              'before_time_zone':['09:30:00','11:30:00'],
              'after_time_zone':['13:00:00','15:00:00'],
              'portfolio': {
                  'positions': {
                      'stocks': ['ltc2l_usdt'],
                      'ltc2l_usdt': {
                          'operation_id':'dc_ltc2l_usdt_mxc008-e-167.ltc2l_usdt',
                          'operation_stamp':'20210112',
                          'trigger_price':-9999,
                          'unit_shares':10,
                          'current_price': -9999,
                          'avg_cost': -9999,
                          'amount': 0,
                          'available_cash': 200,
                          'initial_assgin_cash': 200,
                          'bench':-9999,
                          'getamout': {'maxprice':-9999,'getamout_type':'121',
                          //'maxprice':-9999代表这个选项无效，'getamout_type':'121'，3位数表示
                          //xyz:x代表rate的第一列数据,y表rate的第二列数据，z代表basebuy_price和basesell_price的类型。其中xyz代表的值1代表值，2代表比例。如0.6:0.1,由于'getamout_type':'121'，前两位1和2，那么0.6代表值，0.1代表10%的仓位，即在0.6时仓位为10%，第三位为1则代表basebuy_price和basesell_price为值得设定，
                          其中basebuy_price代表低于这个价格才买，basesell_price代表高于这个价格才卖（其中对应其得初始值在其配置文件，并需同时配置db5的mxcstrategysetting值，然后basebuy_price和basesell_price的值以mxcstrategysetting为准），当为2时，需要结合maxprice配置对应的比例，比如basebuy_price=0.1代表的值为：maxprice*basebuy_price
                                      'rate':{
                                          0.6:0.10,
                                          0.55:0.20,
                                          0.5:0.30,
                                          0.45:0.45,
                                          0.4:0.60,
                                          0.35:0.80,
                                          0.3:1
                                      }
                                      },
                          'bottom_price':-9999,
                          'basebuy_price':0.5,
                          'basesell_price':0.5,
                          'basebuy_rate':-9999,
                          'basesell_rate':-9999,
                          'strategyinfoid':'mxcstrategysetting'
                      }
                  },
                  'cash': -2
              }}  

        3.范围值策略说明
          3.1 最低值和最高值范围，并以多少等分
              'getamout': {'maxprice':-9999,'getamout_type':'121','rate':{'rangevalue':[0.433,0.924,10]}},
              对应的说明:
                  以0.433-0.924,分成10等分，'getamout_type':'12x',所以key为值范围，value为仓位比例值，通过日志可以得到如下
              对应的代码值：
                  initial_getamoutlistkeys:
                  [0.924, 0.8749, 0.8258, 0.7767, 0.7276, 0.6785, 0.6294, 0.5803, 0.5312, 0.4821, 0.433----价格列表
                  initial_getamoutlistvalues:
                  [0.0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0]---仓位比例
                  [2021-06-09 15:30:02,099]root-INFO:
                  getamoutlistkeys:
                  [0.924, 0.8749, 0.8258, 0.7767, 0.7276, 0.6785, 0.6294, 0.5803, 0.5312, 0.4821, 0.433]----价格列表
                  getamoutlistvalues:
                  [0.0, 87.15303867403314, 174.30607734806628, 261.4591160220994, 348.61215469613256, 435.7651933701657, 522.9182320441988, 610.071270718232, 697.2243093922651, 784.3773480662983, 871.5303867403314]----仓位数量



#### **5.1策略使用说明**
      1.策略使用说明:
        策略算法:dc_strategy_grid_F.py
        策略交易模板：dc_product_trade_strategy_grid_F.py
        策略交易模板运行实例：mxc010-btc2l-01-strategy-f-167.py
        
      2.首先复制策略模板，并为策略编号
        策略模板：
          strategy/grid/dc_product_trade_strategy_template/dc_product_trade_strategy_grid_F.py
          mxc010-btc2l-01-strategy-f-167.py(mxc代表抹茶策略，010代表抹茶策略编号，btc2l代表操作品种，f代表策略类别dc_strategy_grid_F)
          其中策略模板dc_product_trade_strategy_grid_F.py和mxc010-btc2l-01-strategy-f-167.py代码一样，mxc010-btc2l-01-strategy-f-167.py仅仅单个运行实例。
#### **5.2备份数据库shell脚本**
    备份数据库shell脚本，放在定时任务里，每天15：05执行脚本（通过宝塔定时任务“定时备份redis的db0”）：
      /sysmedia/usb3hdd1_120g/dev/stockanalyse/experiment/shellscript/bakredisdb.sh
    备份路径：
      /sysmedia/usb3hdd1_120g/dev/stockanalyse/strategy/grid/db/
    格式如下：
      /sysmedia/usb3hdd1_120g/dev/stockanalyse/strategy/grid/db/db0_20201020.json

#### 配置SSH代码环境
      配置SSH代码环境（以及免密登录）
      1.安装插件Remote-SSH
      2.本地windows生成密钥
      打开cmd，然后输入ssh-keygen后一路回车即可
      在路径：C:\Users\dfg14（用户名）\.ssh
      生成id_rsa和id_rsa.pub文件
      3.将id_rsa.pub复制到远程服务器/root/.ssh下（以root用户登录）
      4.然后将id_rsa.pub追加到authorized_keys文件里即可
      cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
      5.SSH服务器配置
      CTRL+SHIFT+P打开SSH的配置文件C:\Users\dfg14（用户名）\.ssh\config
      C:\Users\dfg14（用户名）\.ssh\config
      Host 192.168.10.190
        HostName 192.168.10.190
        User root
        ForwardAgent yes
        IdentityFile C:\Users\dfg14\.ssh\id_rsa
      6.参考文档：
      https://blog.csdn.net/qq_40183281/article/details/107565280

#### 钉钉提醒提醒个功能
      钉钉提醒提醒个功能：
      0.功能性的类放入lib文件夹里，供调用
      1.先运行reminder_subscriber.py（接受消息）
      2.再运行trade_strategy_grid对应的策略文件（这里面的文件发布消息）


## **6. 运维清单**
 

#### **6.1 日志输出**
      日志输出：
      tail -f /media/usb3hdd1_120g/dev/stockanalyse/strategy/grid_product_trade_strategy_grid_A_logout/logs/20201020/202010200827.log 
      tail -f /media/usb3hdd1_120g/dev/stockanalyse/strategy/grid_product_trade_strategy_grid_B_logout/logs/20201020/202010200827.log
 
#### **6.2 运维**
      日志输出：
      开个新的钉钉群，作为系统运行异常提醒的。
      交易群专门作为交易提醒
      查询python3.8的进程
        ps -eo pid,cmd | grep python3.8
      后台运行python程序：
        nohup python3.8 -u close.py >/dev/null 2>&1 &
      查询close.py进程是否运行：
        ps aux |grep 'close.py' | grep -v grep   
      
      杀掉进程：
        kill -9 进程id
      后台运行:
      nohup python3.8 -u product_trade_strategy_grid_A.py >/dev/null 2>&1 &
      nohup python3.8 -u product_trade_strategy_grid_B.py >/dev/null 2>&1 &

      日志输出：
        
      self.fh.setLevel(logging.DEBUG)

      --判断path路径和包版本以及包路径
      import sys
      from import as pd
      sys.path
      pd.__version__
      pd.__path__


      reminder_check_process
      python3.8 -u reminder_check_process.py


      判断运行程序在哪个版本下运行：
        import sys
        print(sys.version)

      ubuntu screen 实用命令
          常用的几个命令：
          screen -S name 启动一个名字为name的screen
          screen -S name -X quit 删除某个session
          screen -ls 是列出所有的screen
          screen -r name或者id，就可以回到某个screen了（如不行先detached： screen -d name）
          ctrl + a + d 可以回到前一个screen，当时在当前screen运行的程序不会停止

      Jupyter 在 Ubuntu 下的使用
        sudo ubuntu
        jupyter notebook
      jupyter配置信息
        vim ~/.jupyter/jupyter_notebook_config.py
        c.NotebookApp.allow_remote_access = True #允许远程连接
        c.NotebookApp.ip='*' # 设置所有ip皆可访问
        c.NotebookApp.password = u'sha:..' #之前复制的密码
        c.NotebookApp.open_browser = False # 禁止自动打开浏览器
        c.NotebookApp.port =8888 #任意指定一个端口
        c.NotebookApp.default_url = '路径'

        实际环境：
          c.NotebookApp.ip = '0.0.0.0'                     
          c.NotebookApp.open_browser = False                  
          c.NotebookApp.port = 9999                     
          c.NotebookApp.notebook_dir = '/media/usb3hdd1_120g/dev/'   


      Ubuntu断网解决办法
      sudo vim /etc/ppp/options
      改lcp-echo-interval 300
      icp-echo-failure 40
      写一段代码，读写硬盘15分钟间隔，然后见监控一下

#### **6.3 日志输出等级**
      1、DEBUG
      面向功能开发人员，一般在功能开发阶段使用，用来给开发人员检查功能是否正常的。
      此类日志只在开发阶段使用，版本不输出。

      2、INFO
      内测 版可输出此日志，用来查看新开发特性或者 bugfix 是否正常。
      上网版不输出。

      3、WARN
      不会对系统造成影响的非正常流程。
      上网版本输出。

      3、ERROR
      会对系统造成影响的非正常流程，可自我修复，不影响系统稳定性。
      上网版本输出。

      5、CRITICAL
      产生了不可逆的错误，系统无法正常工作。
      上网版本输出。

      另：
      日志只可记录系统运行状态，不可输出用户敏感数据。

#### **6.4 日志输出等级**
    获取外网ip：curl www.icanhazip.com

#### **6.5 VSCode 快捷键**
      vscode代码编辑器折叠所有区域的代码快捷键
      查看了使用说明，快捷键如下：
      1. 折叠所有区域代码的快捷键：ctrl+k, ctrl+0;
                        先按下ctrl和K，再按下ctrl和0; (注意这个是零，不是欧)
      2. 展开所有折叠区域代码的快捷键：ctrl +k, ctrl+J;
                        先按下ctrl和K，再按下ctrl和J 
      3. 自动格式化代码的快捷键：ctrl+k, ctrl+f;
                        先按下ctrl和K，再按下ctrl和f;

#### **6.6 ubunutu 防火墙开启和端口开放**
      解决机器无端重启，宝塔端口被墙以后无法正常登录,还有mysql对应的端口进行重新开放，可以通过宝塔面板进行处理。（宝塔和mysql只要机器重启，则端口映射需要重新处理一下）      
      禁止：sudo ufw disable 
      开启：sudo ufw enable
      查看防火墙状态：sudo ufw status
      重启ufw防火墙：sudo ufw reload 
      开放端口：sudo ufw allow 1111/tcp或sudo ufw allow 1111/udp
      关闭端口：sudo ufw delete allow 1111/tcp



#### **6.7 ubunutu 启动命令**
  重启：sudo reboot
  关机：shutdown -h now或halt或poweroff

#### **6.8 flask问题**
  端口被占用，查询7891端口的进程，然后，杀掉
  lsof -i:7891
  kill -9 进程id

  或
  netstat -tunlp
  查看到对应的进程杀掉就可以了
  
#### **6.9 mysql密码修改**
    1.登入
    mysql -u root -p
    输入root密码
    2.切换库
    use mysql
    切换库
    3.更新密码 
    UPDATE user SET password=PASSWORD('Test1xxxx') WHERE user='root';
    FLUSH PRIVILEGES;
    SET PASSWORD FOR root=PASSWORD('Test1xxxxx');
    update mysql.user set authentication_string=password('Test1xxxxx') where user='root' ;
    4.重启mysql

#### **6.10 文本检索**
  ```命令行说明
    fzf：
    安装:sudo apt install fzf
    使用：
    命令行输入fzf
    或fzf --preview 'head -100 {}'

    locate文件检索：
    安装:sudo apt install locate
    更新索引:updatedb
    搜索文件:locate -b -i "*lamport*.pdf*"

    find命令：
    在根文件夹下查找后缀名为txt且含有关键字route的文件，列出文件名和route所在行。
    find / -name '*.txt' | xargs grep 'route'
    表示当前目录下搜索含有Temporary_random内容的所有文件
    find ./ -name "*" | xargs grep "Temporary_random"
    找出日志文件对应的错误关键字
    find /sysmedia/usb3hdd1_120g/dev/digitalcurrencyanalyse/strategy -name '20210725.log' | xargs grep 'CRITICAL'
  ```


#### **6.11 python常见错误**
  ```命令行说明
    1.urllib3.connectionpool-WARNING:Connection pool is full, discarding connection
    解决办法：
    sudo vim lib/python3/dist-packages/requests/adapters.py（针对python3.8 对于ubuntu用户就是python）
    将DEFAULT_POOLSIZE改大一些，因为池子有个大小。设置100就可以了。
  ```

#### **6.12 centos 6.8 http代理服务器**
  ```命令行说明
    注意提醒：tinyproxy有时候不稳定
    [参考网址](https://www.cnblogs.com/pythonClub/p/9879446.html)
    安装tinyproxy
        yum install tinyproxy
        vim /etc/tinyproxy/tinyproxy.conf
          修改Port 端口号为你想设定的值
          将Allow 选项后面的IP改成你想使用这个代理的客户机的IP，如果你想任何人都可以访问，把这行前面加个#注释一下就行了
        使用
        service tinyproxy stop
        service tinyproxy start
        service tinyproxy restart
        来停止、启动、重启tinystart
        将tinyproxy设置开机启动
        chkconfig --level 2345 tinyproxy on
        设置防火墙
        iptables -I INPUT -p tcp --dport 设置的代理端口 -j ACCEPT  && service iptables save &&service iptables restart
        [防火墙](https://cloud.tencent.com/developer/article/1157535)：
          状态
          service iptable status
          临时关闭防火墙
          service iptables stop
          永久关闭防火墙
          chkconfig iptables off

          centos 7.3：
            systemctl stop firewalld.service #停止firewall
            systemctl disable firewalld.service #禁止firewall开机启动
            firewall-cmd --state #查看默认防火墙状态（关闭后显示notrunning，开启后显示running）
        [Centos6.8防火墙配置](https://www.cnblogs.com/xxoome/p/6884376.html)
              1、基本操作
              # 查看防火墙状态
              service iptables status
              # 停止防火墙
              service iptables stop 
              # 启动防火墙
              service iptables start 
              # 重启防火墙
              service iptables restart 
              # 永久关闭防火墙
              chkconfig iptables off 
              # 永久关闭后重启
              chkconfig iptables on　

              2、查看防火墙状态，防火墙处于开启状态并且只开放了22端口
              3、开启80端口
              vim /etc/sysconfig/iptables
              # 加入如下代码，比着两葫芦画瓢 :)
              -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
              保存退出后重启防火墙
              service iptables restart          
              4.通过corntab定时刷新任务
                参考网址：
                  https://blog.csdn.net/xuehaiwuya_1/article/details/53926013
                  https://www.cnblogs.com/linjiqin/p/11720673.html
                  https://blog.csdn.net/mouday/article/details/81981843
                a.编辑时间（表示59分钟刷新一下服务：a1主要方案，a2为参考方案不做主流配置）
                  a1:通过.sh执行命令（https://www.cnblogs.com/syq816/p/8295184.html）
                      a11.新建tinyproxy.sh，并给予权限chmod 777 tinyproxy.sh
                        #！/bin/sh
                        /etc/init.d/tinyproxy restart                       

                      a12:直接文件编辑命令
                        crontab -e命令编辑文件
                          SHELL=/bin/bash
                          PATH=/sbin:/bin:/usr/sbin:/usr/bin
                          MAILTO=root
                          HOME=/
                          */59 *  * * * bash /root/tinyproxy.sh
                  
                  a2：其他方案备注
                    */1 * * * * bash /root/tinyproxy.sh或
                    20   16       *             *     *     root     service tinyproxy restart
                b.常用命令
                  查看当前用户定时任务
                    crontab -l 
                    调用/var/spool/cron/目录下相关用户的定时任务信息
                  查看定时任务日志
                    tail -f /var/log/cron
                  备注：
                    查找服务路径：https://blog.csdn.net/u013078871/article/details/111058802
                    tinyproxy服务路径：/usr/sbin/tinyproxy
                  
        开启路由功能:
          vi /etc/sysctl.conf
          net.ipv4.ip_forward = 0  ，将0改成1
          或echo "1" > /proc/sys/net/ipv4/ip_forward
          sysctl -p  #重启
        配置dns:
          vim /etc/resolv.conf
            nameserver 223.5.5.5
            nameserver 119.29.29.29
            nameserver 180.76.76.76
            nameserver 114.114.114.114
            nameserver 8.8.8.8
        查看日志：
          cat /var/log/tinyproxy/tinyproxy.log
        查看监听端口：
          netstat -ntl
        出现问题：
          让/etc/tinyproxy.conf中的所有端口都通过删除ConnectPort行，或则添加对应的端口
        测试：
          curl -x 127.0.0.1:8888 google.com
        
    python测试代理：
      import requests
      requests.get('http://httpbin.org/ip',proxies={'http':'http://xxx:8888'}).json()
      输出：
          {'origin': '代理ip'} 

    命令行测试代理：
        curl -x xxxxIp:8888 google.com
  ```

#### **6.13 Ubuntu14.04 http代理服务器squid3---废掉不建议使用**
  ```命令行说明
    注意提醒：tinyproxy有时候不稳定
    开启服务：
    service squid start
    停止服务：
    service squid stop
    vim /etc/squid/squid.conf
    vim /etc/squid/squid.conf
    [Ubuntu下搭建高匿HTTP代理（亲测可用）](https://www.cnblogs.com/dadonggg/p/10019026.html)
    [Squid 启动/停止/重载/服务异常排查实用命令](https://blog.51cto.com/u_2333657/2330887)
    [squid：http和https的代理服务器设置指南](https://blog.csdn.net/liumiaocn/article/details/80586879)
    [Squid代理常见错误](https://blog.csdn.net/qq_31666147/article/details/52047358)
    [使用squid代理后某些网站无法访问的解决办法](https://blog.51cto.com/xjsunjie/388405)
    [centos搭建http代理](https://blog.csdn.net/weixin_42081389/article/details/105405148)
  ```
    
#### **6.12 centos 6.8 socks代理服务器**
  ```命令行说明
    注意提醒：
      a.无密码的稳定，带用户密码的不稳定
      b.python调用模块，需要安装pip install requests[socks],对应我们系统：sudo python3.8 -m pip install requests[socks]
          代理调用模式:self.PROXY为ip:端口
              proxies = {'http':"http://{0}".format(self.PROXY), 'https':"https://{0}".format(self.PROXY)} 
              proxies = {'http':"socks5://{0}".format(self.PROXY), 'https':"socks5h://{0}".format(self.PROXY)}    
          [Python设置和HTTP请求socks,Pythonrequests,http,代理](https://www.pythonf.cn/read/160622)

      c.查看运行日志
        cat /var/log/ss5/ss5.log
      d.如安装出现问题，升级一下
          yum -y update：升级所有包同时也升级软件和系统内核；
      e.如果代理测试本机正常127.0.0.1，外网机器不行，那么考虑dns和防火墙问题：
        dns(加入靠谱的dns):
          vim /etc/resolv.conf 
          nameservre 8.8.8.8
          nameservre 8.8.4.4
          service network  restart
      f.查看用户环境
        uname -a
        service tinyproxy status
    参考网址：
      [记一次socks5部署时的坑](https://blog.csdn.net/xiaohuixing16134/article/details/88530213)  
      [安装 SS5 SOCKS5 代理服务器，多进程/多IP地址出口/多端口 ](https://www.huangzhong.ca/zh/ss5-multiple-instances-ips-centos/)  
    安装：
      wget https://nchc.dl.sourceforge.net/project/ss5/ss5/3.8.9-6/ss5-3.8.9-6.tar.gz(原来是版本有问题，最新的ss5-3.8.9-7.tar.gz有问题，装回ss5-3.8.9-6.tar.gz就可以了!  )
      yum -y install gcc automake make
      yum -y install pam-devel openldap-devel cyrus-sasl-devel openssl-devel
      tar xvf ss5-3.8.9-6.tar.gz
      cd ss5-3.8.9
      ./configure && make && make install
    无验证(只需要配置这个文件就够了，风险有可能别人对公网的机器进行扫描用到你的代理，其他没什么)：
        vim /etc/opt/ss5/ss5.conf
          auth 0.0.0.0/0 - -
          permit - 0.0.0.0/0 - 0.0.0.0/0 - - - - -   
    有验证，带账户密码(1.2步骤可以不用做，写在这里作为一个参考，直接配置3就可，这种严重很多浏览器不支持，并且配置以后google很多网站无法访问)：
      1.新建用户和密码
      useradd username
      passwd username
      2.授予普通用户sudo权限
      打开/etc/sudoers文件，在  "root ALL=(ALL) ALL"下面添加一行 "username ALL=(ALL) ALL"或者 "username ALL=(ALL) NOPASSWD: ALL"
      3.配置
        vim /etc/opt/ss5/ss5.conf
          #auth 0.0.0.0/0 - u
          #permit u 0.0.0.0/0 - 0.0.0.0/0 - - - - -
        vim /etc/sysconfig/ss5（将下面注解打开）
           SS5_OPTS=" -u root"
        vim /etc/opt/ss5/ss5.passwd(用户名密码可以设置很多，无需系统的用户名和密码)
          username pwd
          username2 pwd
      开启服务等命令：
        service ss5 stop
        service ss5 start
        service ss5 restart
        检测服务：
              netstat -anp | grep ss5
        随系统启动：
              chmod a+x /etc/init.d/ss5
              chkconfig --add ss5 
              chkconfig --level 345 ss5 on
              service ss5 start
      测试代理：
        检测ip地址区域：
            curl myip.ipip.net
            curl myip.ipip.net --socks5 xx.xx.xx.xx:1080
            或curl --socks5 127.0.0.1:1080 http://google.com/            
        带验证:
          curl myip.ipip.net --socks5 test:test@xx.xx.xx.xx :1080 
          或curl --socks5 user:pwd@127.0.0.1:1080 http://google.com/ 
  ```     



#### **6.14 shadowsocks 服务**    
    [自己搭建翻墙服务器 ](https://jiyiren.github.io/2016/10/06/fanqiang/)
    vim /etc/ssh/sshd_config
#### **6.15 linux 远程端口22修改** 
    [Linux 修改远程默认端口](https://blog.csdn.net/little_skeleton/article/details/81075065)

    
#### **6.16 树莓派4b+64位ubuntu mongodb**
  ```命令行说明
  参考：
  Raspberry Pi 筆記(69)：安裝mongoDB資料庫與mongo-express網頁管理工具
  https://atceiling.blogspot.com/2020/03/raspberry-pi-69mongodbmongo-express.html

  1.如果以前安装了的，先卸载干净
  $ sudo service mongod stop#停掉服务
  $ sudo apt-get purge mongodb mongodb-clients mongodb-server mongodb-dev
  $ sudo apt-get purge mongodb-org*
  $ sudo apt-get autoremove
  #删除数据库和日志文件
  $ sudo rm -r /var/log/mongodb
  $ sudo rm -r /var/lib/mongodb
  sudo vim /etc/mongodb.conf

  2.安装
  如果安装不上，可以先升级系统，如果系统需要翻墙，
  临时使用export http_proxy=http://xx.xxx.xxx.xxxx:8888，
  进行代理访问外网
  $ sudo apt-get update && sudo apt-get upgrade
  安裝MongoDB
  $ sudo apt-get install mongodb-server
  安裝完成後，可以執行以下指令進入 mongoDB 的指令模式：
  $ mongo如要查看 mongoDB 的狀態，可執行：
  $ sudo service mongodb status

  服务命令：
  $ sudo service mongodb start
  $ sudo service mongodb stop
  $ sudo service mongodb restart
  exit：退出mongo

  3.用户认证访问：
  设置用户访问
  sudo vim /etc/mongodb.conf修改配置
  bind_ip = 0.0.0.0#
  auth = true

  
  进入命令行：
  mongo
  #创建用户密码:
  use admin
  db.createUser(
    {
      user: "username",
      pwd: "pwd",
      roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
    }
  )

  重启服务
  sudo service mongodb restart

  注意事项：
  a.用户密码验证时
      启用认证:root@ubuntu:/usr/bin#./mongod --auth
      运用3T客户端给用户加上角色:
          "roles" : [
        {
            "role" : "enableSharding", 
            "db" : "admin"
        }, 
        {
            "role" : "dbOwner", 
            "db" : "admin"
        }, 
        {
            "role" : "restore", 
            "db" : "admin"
        }, 
        {
            "role" : "clusterManager", 
            "db" : "admin"
        }, 
        {
            "role" : "dbAdminAnyDatabase", 
            "db" : "admin"
        }, 
        {
            "role" : "clusterAdmin", 
            "db" : "admin"
        }, 
        {
            "role" : "dbAdmin", 
            "db" : "admin"
        }, 
        {
            "role" : "readWriteAnyDatabase", 
            "db" : "admin"
        }, 
        {
            "role" : "userAdminAnyDatabase", 
            "db" : "admin"
        }, 
        {
            "role" : "hostManager", 
            "db" : "admin"
        }, 
        {
            "role" : "userAdmin", 
            "db" : "admin"
        }, 
        {
            "role" : "root", 
            "db" : "admin"
        }
    ]
  b.为了安全性，在防火墙设置ip访问限制即可+账户验证，则可保证安全。
  c.查看进程:ps -ef | grep mongo或 pgrep mongo -l或则通过htop
  d.find / -name "mongodb" 查询目录

  4.客户端链接工具：
  NoSQLBooster for MongoDB
  https://www.jianshu.com/p/ebd0caa3f1ec

  [studio 3T 两种破解教程](https://www.jianshu.com/p/7257f15e2620?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes)
  https://robomongo.org/
  ```

#### **6.17 ubuntu设置http代理服务器**
  ```命令行说明
  局部代理：命令行内有效
      export http_proxy=http://yourproxyaddress:proxyport

  全局代理：
      gedit ~/.bashrc
      http_proxy=http://yourproxyaddress:proxyport
      export http_proxy
      source ~/.bashrc#生效
  ```

#### **6.18 树莓派 Ubuntu无端重启可能原因**

  ```命令行说明
    查看日志：
    cd /var/log
    find / -name '*.log' | xargs grep 'reboot'
    1.linux redis WARNING overcommit_memory is set to 0! 解决方案
      解决办法：编辑/etc/sysctl.conf ，改vm.overcommit_memory=1，然后sysctl -p 使配置文件生效
  ```

#### **6.19 树莓派 Ubuntu系统压缩备份**

  ```命令行说明
    树莓派系统的安装、备份与还原 SD卡分区介绍
    https://www.bilibili.com/video/BV1pK4y1Q7h8/
  ```


#### **ubuntu-002开启jupyter服务**
    1.su ubuntu
    2.screen -S jupyterscreen
    3.jupyter notebook
    4.快捷键ctrl + a + d
    5.访问http://192.168.xxx.xxx:9999/tree



#### **开启小米球服务-发布https服务，通过小米球映射到外网***
    1.screen运行小米球，httpstun7889对应的7889端口，需要防火墙打开
    screen -S dataanalyse #启动一个名字为name的screen
    cd /home/ubuntu/dev/linux_arm7
    sudo ./ngrok -config ngrok.conf -log=ngrok.log start httpstun7889
    参考地址：http://ngrok.ciqiuwl.cn/

    2.运行/product/usb3hdd1_3t/dev/digitalcurrency-dataanalyse/prechart0.5/app.py的flask服务
    https://dataanalyse.guyubao.com/pork

    3.ubuntu screen 实用命令
        常用的几个命令：
        screen -S name 启动一个名字为name的screen
        screen -S name -X quit 删除某个session
        screen -ls 是列出所有的screen
        screen -r name或者id，就可以回到某个screen了（如不行先detached： screen -d name）
        ctrl + a + d 可以回到前一个screen，当时在当前screen运行的程序不会停止

## **树莓派Ubuntu20+chrome+selenium部署**
    0.支持库安装
        sudo apt-get install xvfb
        sudo python3.8 -m pip install selenium
        sudo python3.8 -m pip install pyvirtualdisplay
        查询版本
        sudo python3.8 -m pip show selenium
        参考：
        https://www.cnblogs.com/x54256/p/8403864.html
        https://blog.csdn.net/qq_31010925/article/details/110308465

    1.ubuntu arm64下载地址
    https://launchpad.net/~canonical-chromium-builds/+archive/ubuntu/stage/+packages
    下载如下安装包到指定地址，然后运行下面4个文件，如果运行错误，则运行一下sudo apt-get install -f，注意顺序
    （如果提示错误dpkg: 依赖关系问题使得xxxxx的配置工作不能继续：则运行sudo apt install -f自动加载依赖包
    之后在运行第二步的命令安装，就能成功）
    sudo dpkg -i chromium-codecs-ffmpeg-extra_90.0.4430.93-0ubuntu0.16.04.1_arm64.deb
    sudo dpkg -i chromium-codecs-ffmpeg_90.0.4430.93-0ubuntu0.16.04.1_arm64.deb
    sudo dpkg -i chromium-browser_90.0.4430.93-0ubuntu0.16.04.1_arm64.deb
    sudo dpkg -i chromium-chromedriver_90.0.4430.93-0ubuntu0.16.04.1_arm64.deb
    其中卸载类似如下
    sudo dpkg -r chromium-codecs-ffmpeg-extra_91.0.4472.114-0ubuntu0.18.04.1_armhf.deb
    sudo dpkg -r chromium-codecs-ffmpeg_91.0.4472.114-0ubuntu0.18.04.1_armhf.deb
    sudo dpkg -r chromium-browser_91.0.4472.114-0ubuntu0.18.04.1_armhf.deb
    sudo dpkg -r chromium-chromedriver_91.0.4472.114-0ubuntu0.18.04.1_armhf.deb 

    2.加入到环境变量
    sudo vim ~/.bashrc
    export PATH=$PATH:/usr/lib/chromium-browser/
    source .bashrc
    加入到

    3.查看chrome版本
    /usr/bin/chromedriver --version

    4.查看安装路径
    dpkg -L chromium-chromedriver

    5.selenium应用
    selenium之CSS定位汇总
    https://www.cnblogs.com/zuodaozhudemeng/p/7487798.html
    selenium 元素查找与属性
    https://www.cnblogs.com/hao2018/p/11285827.html

    6.查看进程工具htop

    7.dpkg 
        查看系统中软件包nano的状态, 支持模糊查询:（l的意思是list）
        $ dpkg -l nano
        我个人经常用上面这句话看状态。

        查看软件包nano的详细信息:
        $ dpkg -s nano

        查询系统中属于nano的文件:
        $ dpkg-query -L nano
        $ dpkg -l libxss1 libappindicator1 libindicator7
        $ dpkg -l xvfb
        $ dpkg -l unzip

    8.wget
        a.下载后，用mv移动
        wget https://launchpad.net/~canonical-chromium-builds/+archive/ubuntu/stage/+files/chromium-browser_90.0.4430.93-0ubuntu0.16.04.1_arm64.deb
        sudo mv chromium-browser_90.0.4430.93-0ubuntu0.16.04.1_arm64.deb /media/usb3hdd1_3t/software/chrome/ubuntu/ubuntu-arm64-9.0/
        b.直接指定下载路径
        wget -P /root/download https://launchpad.net/~canonical-chromium-builds/+archive/ubuntu/stage/+files/chromium-browser_90.0.4430.93-0ubuntu0.16.04.1_arm64.deb

## **dock+树莓派ubuntu+vpn(v2ray)部署**
  ```命令行说明
      主要参考网址: https://github.com/twotreesus/V2ray.FunPi
      0.通过宝塔安装Dock管理器3.7，然后点击设置
          a.镜像管理-获取镜像-点击公共库，然后复制如下镜像，然后等待一会等镜像拉取完成
          raydoom/v2ray-funpi:latest
      1.创建容器，并选好刚才的v2ray-funpi镜像，然后配置对那个的服务器和容器的端口映射（此镜像只提供了socks的访问服务），内存分配1900m
        1080-1080（容器socks服务）
        1086-1086（容器web访问服务）
        a.处理镜像端口映射时出现的问题
            创建失败!500 Server Error: Internal Server Error ("driver failed programming external connectivity on endpoint frosty_nash (8d99a12fdc8e635ac435378ef08d6f1dae8126d5f75b0eab173705ceffed907a): (iptables failed: iptables --wait -t filter -A DOCKER ! -i docker0 -o docker0 -p tcp -d 172.17.0.2 --dport 1086 -j ACCEPT: iptables: No chain/target/match by that name. (exit status 1))")
            解决方案：参考http://blog.sina.com.cn/s/blog_8e032fb90102xuon.html
              执行如下命令：
                pkill docker
                iptables -t nat -F
                ifconfig docker0 down
                brctl delbr docker0
                docker -d
                systmctl restart docker或service docker restart重启docker即可
          b.然后访问http://xxxx.xx.xxx.191/1086就可访问，但需要服务器放行1086和1080端口 
      2.v2ray配置
           进入http://xxxx.xx.xxx.191:1086/，默认账号密码admin/admin
           a.点击页面订阅配置，点击最近订阅的订阅然后输入如下订阅地址，点击ok
            去自己微信收藏中查询，【vpn订阅地址】标签（即通过https://www.zhixin66.com/aff.php?aff=2476购买的v2rayN.exe对应的服务器代理的订阅地址链接）
           b.然后获取最新最快的站点，点击打勾按钮应用即可，一般选择日本、新加坡   
           c.运行状态中，选择代理模式：
              全局代理:就是所有代理都通过代理服务器上网
              智能分流：根据网址智能区分走代理还是直接本国链接
      3.使用dock搭建的sock代理或程序中使用代理
          curl myip.ipip.net --socks5 xxxx.xx.xxx.191:1080
          curl google --socks5 xxxx.xxx.xxx.191:1080
          如检测代理ip：
            curl myip.ipip.net --socks5 xxxx.xx.xxx:1080
            curl --socks5 xxxx.xx.xxx:1080 http://www.cip.cc/
            curl --socks5 xxxx.xx.xxx:1080 http://httpbin.org/get
            curl --socks5 xxxx.xx.xxx:1080 http://myip.ipip.net
      4.其他
      因为自己再部署191机器时候部署v2rayClient（https://github.com/NoOne-hub/v2ray_client）没有部署成功，所以有残余进程
      开机后supervisorctl stop v2rayClient停止进程。

      如果配置正确还访问不了，就换一下国外dns，最好哪个地方的ip配置哪个地方的dns，比如我用新加坡的ip就配置新加披的dns:
        国内DNS
            114DNS：114.114.114.114、114.114.115.115
            阿里云DNS：223.5.5.5、223.6.6.6
            腾讯DNS：119.29.29.29、119.28.28.28
            百度DNS：180.76.76.76
            CNNICDNS：1.2.4.8
            360DNS：101.226.4.6
        国外DNS
          新加坡DNS：202.136.162.11
          谷歌DNS：8.8.8.8、8.8.4.4
          CloudflareDNS：1.1.1.1、1.0.0.1
          IBM DNS：9.9.9.9
          OpenDNS：208.67.222.222、208.67.220.220          
          中国台湾：101.101.101.101
  ```

## **2012 server 配置asp环境**
  ```命令行说明
  2012 server配置iis8--asp环境
    1.安装数据库引擎
    64位引擎：AccessDatabaseEngine.exe（siweiwo/resource/tech/computer/software/syssoftware/AccessDatabaseEngine.exe）    
    https://blog.csdn.net/a237428367/article/details/9272361

    2.将所有Microsoft.Jet.OLEDB.4.0 改成Microsoft.ACE.OLEDB.12.0

    3.配置iis8服务器
    windows server 2012 IIS8.0配置
    https://jingyan.baidu.com/article/b24f6c82c504d686bfe5da3d.html

    4.配置asp环境
    Windows server 2012 IIS 安装asp网站
    https://blog.csdn.net/weixin_45142980/article/details/105472812

    5.权限配置
    因为access解压临时文件时候需要用到临时文件夹，如果没有权限则无法正常运行：
    C:\Users\Administrator\Local Settings\temp
    C:\Windows\Temp
    两个目录加入IIS_IUSRS用户权限

    6.防火墙
    腾讯云服务器操作系统内端口开放：
    控制面板-系统和安全-windows防火墙-高级设置-入站规则
    新建入站规则，开放对应的端口。

    腾讯云服务器web管理页面配置，服务器系统外防火墙端口开放：
    进入轻量应用服务器---防火墙---添加规则
    TCP对应的端口开放

    7.访问
    http://外网ip:端口/personalmgmt/index.asp
  ```



#### **ubuntu-002开启文件同步命令**

#### **ubuntu-002计划任务**
    文件每周备份任务

#### **gitee:vs code 代码托管到gitee**
    1.创建仓库
      https://gitee.com/codegalary/digitalcurrencyanalyse
    2.创建用户和公钥
      su root
      ssh-keygen -t rsa -C "xxxxx@gmail.com"
      回车
    3.查看公钥
      cat ~/.ssh/id_rsa.pub
    4.打开码云，将公钥加入
      https://gitee.com/profile/sshkeys
      ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDMN351uFKvuDQhnV+Gg8+NIFHySwnpiRMdIe3JJ/zqQo+D6v4DmveXHzO729yT+guy9Inl/2xavAziiQneKaqkMrKUDmURT30bj6uRmXP/7RhXih8+7qWXfdd/C0
    5.验证校验通过
      ssh -T git@gitee.com
      选择yes
      # Welcome to Gitee.com, YourGiteeName!
      6.初始化git
      git config --global user.name pi4_001_ubuntu_root
      git config --global user.email xxxxx@gmail.com
      7.创建仓库
      在vscode上关键仓库
      或则目录行下输入
      git init
    8.关联远程仓库
      $ git remote add origin git@gitee.com:xxxxx/xxxxx.git
    9.提交
      9.1.点击vscode source control,并将要添加的文件选中，然后点击添加
      类似命令git add .
      9.2.然后在命令行提交
      git commit -m "说明"
      9.3.然后输入命令，本地提交到远程
      git push -f origin master  //强制本地push过去
    10.标签命令
    查看提交
    git log --pretty=oneline --abbrev-commit
    创建标签
    git tag -a v1.0.1 -m "version 1.0.1 released" //描述
    git tag -a v1.0.1 -m "version 1.0.1 released" 63efdfc//指定提交版本加标签
    删除标签
    git tag -d 标签名
    提交标签
    git push origin --tags
    提交具体标签
    git push origin 标签名

    11.删除git库
    递归清除本地文件夹下的Git文件，如果想重新建立仓库，那么在重新初始化新建的git仓库
    //删除文件夹下的所有 .git 文件
    find . -name ".git" | xargs rm -Rf
    //初始化仓库
    git init
#### **ubuntu压缩和解压**
    1.压缩当前目前下的【全能行证券交易终端】目录下的所有文件---》全能行证券交易终端.tar.gz
      tar -zcvf 全能行证券交易终端.tar.gz 全能行证券交易终端
    2.解压
      tar -C DesDirName -zxvf FileName.tar.gz # 解压到目标路径
    3.备注
      Linux之tar常用的几种情况，如下所示：
          tar压缩：
          tar -czvf  filename.tar.gz filename
          tar解压
          tar -xzvf filename.tar.gz  -C 解压路径
          tar加密压缩：
              tar czvf - filename |openssl des3 -salt -k password | dd of=filename.tar.gz
          tar解密解压：
              dd if=filename.tar.gz | openssl des3 -d -k password | tar xzvf - -C解压路径
          password为密码自行设定，出现如下错误，一般是需要通过-C来指定解压路径：
#### **ubuntu7z的压缩和解压**
    1.7z的解压缩
    2.ubuntu下安装7z
    3.7z压缩和解压
      压缩：
      7z a -t7z product/usb3hdd1_3t/computerbak/surface_import/2/zhanghu.7z -pxxxx product/usb3hdd1_3t/computerbak/surface_import/2/zhanghu.txt
      解压：
      7z x product/usb3hdd1_3t/computerbak/surface_import/2/zhanghu.7z -pxxxx
      其中-p后面的xxxx即为解压缩密码
    4.7z命令可以看到对一个的参数说明
    5.建议采用定位到目录以后在操作
      cd /product/usb3hdd1_3t/computerbak/surface_import/2
      7z a -t7z zhanghu.7z -pTestxxxxx zhanghu.txt
      7z x zhanghu.7z -pTestxxxxx

#### **文件对应关系**
scp -r /media/usb3hdd1_120g/dev/ root@192.168.10.191:/product/usb3hdd1_3t/dev/
scp -r /media/usb3hdd1_120g/computerbak/ root@192.168.10.191:/product/usb3hdd1_3t/
#### **Ubuntu ssh服务器copy**
    1.两台linux服务器之间免密scp，在A机器上向B远程拷贝文件
      操作步骤：
      1、在A机器上，执行ssh-keygen -t rsa，一路按Enter，不需要输入任何内容。（如有提示是否覆盖，可输入y后按回车）
      2、到/root/.ssh/目录下，查看是否有id_rsa.pub文件生成
      3、将A机器生成的id_rsa.pub文件拷贝到B机器的/root/.ssh/下，并将id_rsa.pub改名为authorized_keys（如果B机器已经有了authorized_keys，可以编辑，向下追加ssh-rsa内容即可）
      4、好了，现在就可以在A机器上通过scp aaaa.txt root@172.16.1.2:/home/xxx/ 命令进行免密远程拷贝了
      注意：
      1、A机器修改密码后，需要重新按上述步骤操作一遍
      2、复制的两台计算机需要用相同的账户名 
      copy整个目录（直接覆盖，多余的文件不会删除），
      scp -r /media/usb3hdd1_120g/dev/ root@192.168.10.191:/product/usb3hdd1_3t/
      Copy 文件
      scp /media/usb3hdd1_120g/dev/file root@192.168.10.191:/product/usb3hdd1_3t/dev/

#### **机器部署规划**
    0：机器说明
      ubuntu_001：树莓派ubuntu机器策略交易，配置4核+4G+固态500g系统盘+外接120G外置移动硬盘
      ubuntu_002：树莓派ubuntu机器策略分析，配置4核+8G+固态500g系统盘+外接3T外置机械硬盘
    1.ubuntu_001策略交易代码运行在固态硬盘上
    2.ubuntu_001机器
      4核4g 策略运行机器
        /media/usb3hdd1_120g/dev
        /sysmedia/usb3hdd1_120g/dev/
      原有磁盘开发和生产目录（可以认为代码备份，日期为20210609）：
        /media/usb3hdd1_120g/dev
      现有开发和生产目录同一个：
        /sysmedia/usb3hdd1_120g/dev/
      copy同步目录：
        scp -r /media/usb3hdd1_120g/dev/ root@192.168.10.190:/sysmedia/usb3hdd1_120g/
        同步digitalcurrencyanalyse文件夹：
        scp -r /sysmedia/usb3hdd1_120g/dev/digitalcurrencyanalyse/ root@192.168.10.190:/media/usb3hdd1_3t/ubuntu_001_bak/usb3hdd1_120g/dev/

      定期备份（scp配置ssh，这样copy时不需要密码验证参考：Ubuntu ssh服务器copy）
        /sysmedia/usb3hdd1_120g/dev/定期将对应的代码备份到/media/usb3hdd1_3t/ubuntu_001_bak
        备份全量dev:直接覆盖
        scp -r /sysmedia/usb3hdd1_120g/dev/ root@192.168.10.191:/media/usb3hdd1_3t/ubuntu_001_bak/usb3hdd1_120g/



        定时任务备份代码：
        定时备份数字货币交易策略代码(在宝塔面板的计划任务，创建计划任务，将该命令加入进来)：
        scp -r /sysmedia/usb3hdd1_120g/dev/digitalcurrencyanalyse root@192.168.10.191:/media/usb3hdd1_3t/ubuntu_001_bak/usb3hdd1_120g/dev/
    2.ubuntu_002
        product目录为固态硬盘目录，主要运行策略程序，因为代码大部分可以服用，故ubuntu_002的代码复制了一套到ubuntu_001
        /product/usb3hdd1_3t/dev/digitalcurrencyanalyse为20210609的一个版本，里面会有有些案例代码experiment
        对应的压缩代码在ubuntu_001的/sysmedia/usb3hdd1_120g/dev/digitalcurrencyanalyse/experiment是一样的

