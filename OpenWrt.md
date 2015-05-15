##安装编译需要的组件
    yum install subversion binutils bzip2 gcc gcc-c++ gawk gettext flex ncurses-devel zlib-devel make patch unzip perl-ExtUtils-MakeMaker  glibc glibc-devel glibc-static quilt ncurses-lib sed sdcc intltool sharutils bison wget git-core openssl-devel xz 
##源码下载
    cd ~
    //最新源码参考 https://dev.openwrt.org/wiki/GetSource
    git clone git://git.openwrt.org/14.07/openwrt.git
##获取编译配置文件
    cd ~/openwrt
    //应该根据路由器型号及源码版本选择相应的配置文件
    //openwrt支持的路由器及芯片参考 http://wiki.openwrt.org/toh/start
    wget https://downloads.openwrt.org/barrier_breaker/14.07/ramips/rt305x/config.ramips_rt305x
    ln -s config.ramips_rt305x .config
Now edit the .config: set the CONFIG_SDK=n and CONFIG_MAKE_TOOLCHAIN=n to not build the SDK or the TOOLCHAIN.
##包处理
安装包：创建~/openwrt/config_install_feeds.pl
    #!/usr/bin/perl

    use warnings;
    use strict;

    open(CONFIG_FILE, '<', ".config");
    while(<CONFIG_FILE>)
    {
        if( $_ =~ /^CONFIG_PACKAGE_(.+?)=y/)
        {
            my $feed = $1;
            print("./scripts/feeds install $feed\n");
            system("./scripts/feeds install $feed");
        }
    }
运行：

    cd ~/openwrt
    chmod +x config_install_feeds.pl
    ./config_install_feeds.pl
禁用不必要的包：创建~/openwrt/config_feeds_disable_packages.pl
    #!/usr/bin/perl

    use warnings;
    use strict;

    open(CONFIG_FILE, '<', ".config");
    while(<CONFIG_FILE>)
    {
        chomp;
        if( $_ =~ /^CONFIG_PACKAGE_(.+?)=m$/)
        {
            print("CONFIG_PACKAGE_$1=n\n");
        }
        else
        {
            print("$_\n");
        }
    }
运行：

    cd ~/openwrt
    chmod +x config_feeds_disable_packages.pl
    ./config_feeds_disable_packages.pl > .config.new
    mv .config.new .config
##使配置文件生效
    make oldconfig
It will ask questions, answer 63 (or whatever number corresponds to your device) to say we are building for e.g. a TP-Link WR741ND.

Answer n for all the rest of the questions, to not include any packages we did not install.
##选择路由器型号
make menuconfig -> Target Profile -> 选择 HuaWei HG255D
##打开对HG255D板的支持
在target/linux/ramips/image/Makefile中，去掉下面两行行首的#，增加对HG255D编译的支持（用HG255D搜索即可）

    Image/Build/Profile/HG255D=$(call BuildFirmware/Default16M/$(1),$(1),hg255d,HG255D)
    $(call Image/Build/Profile/HG255D,$(1))
##使用原机的mac地址
在target/linux/ramips/base-files/lib/ramips.sh中查找HG255D，能查到如下代码：

    *"HG255D")       
        name="hg255d"
        ;;
将其中的"HG255D"修改为"HuaWei HG255D"，如下：

    *"HuaWei HG255D")
        name="hg255d"
        ;;
##改电源指示灯不能正常工作的问题
在 target/linux/ramips/base-files/etc/diag.sh 中查找：

    hlk-rm04)
        status_led="hlk-rm04:red:power"
        ;;
其下添加：

    hg255d)
        status_led="hg255d:power"
        ;;
##修改缺省lan ip
修改package/base-files/files/lib/functions/uci-defaults.sh

    set network.lan.ipaddr='192.168.2.1'
##打开wlan的支持，并添加无线密码174258o
修改package/kernel/mac80211/files/lib/wifi/mac80211.sh

    option disabled 0
    option encryption 'psk2'
    option key '174258o'
##添加root帐号密码
修改package/base-files/files/etc/shadow文件

    root::0:0:99999:7:::
改成（密码是admin）

    root:$1$21u5EotL$B9ebsVgEQe.C7lsk0iMf10:0:0:99999:7:::
##修改主机名，设定时区
package/base-files/files/etc/config/system

    //设定主机名
    option hostname Openwrt 
    //时区设置为亚洲/上海
    option zonename Asia/Shanghai 
    //正8区
    option timezone CST-8
其中 list server 为ntp服务器
##编译
    make V=s
