---
layout: post
title: 树莓派打造量化平台
date: 2021-07-06
categories: blog
tags: [ubuntu,树莓派,数字货币,量化,股票]
description: 树莓派打造量化平台
---


# digitalcurrencyanalyse

## **1. 数字货币自动交易策略框架**

数字货币自动交易策略框架,通过对接交易所API配合自己的交易策略进行自动化交易

## **2. 软件架构**
  技术和工具：Python3.8+钉钉+微信+redis+宝塔

  树莓派服务器：
  
    ubuntu-001：策略交易服务器
    ubnutu-002：策略分析服务器+数据库服务器

## **3. 安装教程**

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
    1.fzf
    安装:sudo apt install fzf
    使用：
    命令行输入fzf
    或fzf --preview 'head -100 {}'
    2.locate文件检索
    安装:sudo apt install locate
    更新索引:updatedb
    搜索文件:locate -b -i "*lamport*.pdf*"
    3.find命令
    3.1.在根文件夹下查找后缀名为txt且含有关键字route的文件，列出文件名和route所在行。
    find / -name '*.txt' | xargs grep 'route'
    3.2.表示当前目录下搜索含有Temporary_random内容的所有文件
    find ./ -name "*" | xargs grep "Temporary_random"
    3.3.找出日志文件对应的错误关键字
    find /sysmedia/usb3hdd1_120g/dev/digitalcurrencyanalyse/strategy -name '20210725.log' | xargs grep 'CRITICAL'

#### **6.11 python常见错误**
    1.urllib3.connectionpool-WARNING:Connection pool is full, discarding connection
    解决办法：
    sudo vim lib/python3/dist-packages/requests/adapters.py（针对python3.8 对于ubuntu用户就是python）
    将DEFAULT_POOLSIZE改大一些，因为池子有个大小。设置100就可以了。

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

