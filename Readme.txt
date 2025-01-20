用途：用于一键更新macos下所有APP。

-------------------------------------------
使用方法：
-------------------------------------------
1. 下载usercmd_updatefull
2. 在桌面 创建 “快捷方式”（点击后调用本脚本）
3，如果要每日自动执行，建议使用 Lingon X（或类似软件），将usercmd_updatefull挂载为循环任务（比如每天夜间3点钟执行）


-------------------------------------------
日志保存在哪？
-------------------------------------------
 - 每次执行完成后，会自动打开日志目录（如果不希望自动打开，可以注释掉脚本最后一行的代码）


-------------------------------------------
可以更新哪些程序？
-------------------------------------------
 - 本脚本会，调用以下个程序，完成自动化更新
	- HomeBrew	：更新 Homebrew下载的APP
	- Mas		：更新 App Store下载的APP
	- MacPorts	：更新 MacPorts下载（Linux移植）的APP
	- TopGreade	：更新 复查上述在线安装的APP
	- MacUpdater	：更新 下载后离线安装的APP


-------------------------------------------
如何设置定时更新？
-------------------------------------------
 - 建议使用 Lingon X（或类似软件），将usercmd_updatefull挂载为循环任务（比如每天夜间3点钟执行）


-------------------------------------------
如何避免密码请求？
-------------------------------------------
 - 可以通过类似于（  brew upgrade --greedy --debug  ）命令，查看是哪些程序触发了密码请求，
 - 然后将对应的命令行，加入到sudo visudo（或直接删除对应的无用程序）

如：

# 免密更新  MacPorts 
Cmnd_Alias MACPORTS_CMDS = \
    /opt/local/bin/port selfupdate*, \
    /opt/local/bin/port sync*, \
    /opt/local/bin/port upgrade*, \
    /opt/local/bin/port clean*, \
    /opt/local/bin/port outdated*, \
    /opt/local/bin/port rev-upgrade*, \
    /opt/local/bin/port installed inactive*, \
    /opt/local/bin/port uninstall inactive*, \
    /opt/local/bin/port -u selfupdate*, \
    /opt/local/bin/port -u sync*, \
    /opt/local/bin/port -u upgrade*, \
    /opt/local/bin/port -u clean*, \
    /opt/local/bin/port -u outdated*, \
    /opt/local/bin/port -u rev-upgrade*, \
    /opt/local/bin/port -u installed inactive*, \
    /opt/local/bin/port -u uninstall inactive*,\
    /opt/local/bin/port -N reclaim*

你的用户名 ALL=(ALL) NOPASSWD: MACPORTS_CMDS


# 免密更新  homebrew 
Cmnd_Alias BREW_UPGRADE_CMDS = \
    /opt/homebrew/bin/brew upgrade *, \
    /opt/homebrew/bin/brew upgrade --force *, \
    /opt/homebrew/bin/brew upgrade --greedy *, \
    /opt/homebrew/bin/brew upgrade --force --greedy *, \
    /opt/homebrew/bin/brew upgrade --force --greedy --verbose *, \
    /usr/local/bin/brew upgrade *, \
    /usr/local/bin/brew upgrade --force *, \
    /usr/local/bin/brew upgrade --greedy *, \
    /usr/local/bin/brew upgrade --force --greedy *, \
    /usr/local/bin/brew upgrade --force --greedy --verbose *, \
    /usr/bin/launchctl remove *, \
    /usr/bin/launchctl list *, \
    /usr/bin/launchctl unload *, \
    /bin/launchctl remove *, \
    /bin/launchctl list *, \
    /bin/launchctl unload *, \
    /usr/bin/xargs -0 -- /bin/rm -r -f *

你的用户名 ALL=(ALL) NOPASSWD: SETENV: BREW_UPGRADE_CMDS


# 注意：/usr/bin/sudo -E -- 等前缀不要添加到免密命令中，添加后会造成无法匹配的情况

# Office
你的用户名 ALL=(ALL) NOPASSWD: /bin/launchctl list com.microsoft.office.licensingV2.helper
你的用户名 ALL=(ALL) NOPASSWD: /bin/rm -f -- /Library/LaunchDaemons/com.microsoft.office.licensingV2.helper.plist

# Zoom
你的用户名 ALL=(ALL) NOPASSWD: /bin/launchctl list us.zoom.updater
你的用户名 ALL=(ALL) NOPASSWD: /bin/rm -f -- /Library/LaunchAgents/us.zoom.updater.plist

# AirParrot
你的用户名 ALL=(ALL) NOPASSWD: /usr/sbin/kextstat -l -b /Library/Extensions/AirParrotDriver.kext

# OpenVPN
你的用户名 ALL=(ALL) NOPASSWD: /bin/launchctl list org.openvpn.*
你的用户名 ALL=(ALL) NOPASSWD: /bin/launchctl remove org.openvpn.*
你的用户名 ALL=(ALL) NOPASSWD: /usr/sbin/pkgutil --forget org.openvpn.*

# ForkLift
你的用户名 ALL=(ALL) NOPASSWD: /bin/launchctl list com.binarynights.ForkLift*
你的用户名 ALL=(ALL) NOPASSWD: /bin/launchctl remove com.binarynights.ForkLift*



-------------------------------------------
如何检测visudo是否生效
-------------------------------------------
sudo -l


-------------------------------------------
如何查看visudo配置文件
-------------------------------------------
sudo cat /etc/sudoers



-------------------------------------------
如何编辑visudo配置文件 
-------------------------------------------
sudo visudo
注意，退出保存时，新配置回立刻生效，无需手动reload 



-------------------------------------------
特别提示： 
-------------------------------------------
在编辑visudoers时，一定 一定 一定 一定 不要修改如下配置内容！！！
root            ALL = (ALL) ALL
不然不可能要要面临重装系统！



-------------------------------------------
声明：
-------------------------------------------

 - 本脚本所有代码均来自于ChatGPT，且经过长期测试。本工程使用MIT协议分发
