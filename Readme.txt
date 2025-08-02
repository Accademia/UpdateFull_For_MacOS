===========================================
功能与目的：
===========================================
 一键更新macos下所有APP，包括：
  + 第三方APP
  + App Store内的APP
  + 通过Homebrew安装的APP
  + 从Linux移植到Mac的APP
在更新后，自动还原LaunchPad（启动台）布局



===========================================
可以更新哪些程序？
===========================================
 - 本脚本会，调用以下个程序，完成自动化更新
	- HomeBrew	：更新 Homebrew下载的APP
	- Mas		：更新 App Store下载的APP
	- MacPorts	：更新 MacPorts下载（Linux移植）的APP
	- TopGreade	：更新 复查上述在线安装的APP
	- MacUpdater	：更新 下载后离线安装的APP



===========================================
使用方法：
===========================================
1. 下载usercmd_updatefull
2. 在桌面 创建 “快捷方式”（点击后调用本脚本）
3，请将命令拷贝至：/usr/local/bin目录中（或将文件所在的路径，加入到环境变量的$PATH中）
4. 使用usercmd_updatefull命令，启动脚本
5. 如果要每日自动执行，建议使用 Lingon X（或类似软件），将usercmd_updatefull挂载为循环任务（比如每天夜间3点钟执行）

注意:
1. 本脚本会检测，目标网站是否能正常访问，如果不能正常访问，建议开启VPN，并将目标网站挂载到黑名单中。
2. 本脚本，在凌晨 2:50 - 3:00之间，拒绝执行（因为：假定 本脚本会在3:00自动执行）



===========================================
日志保存在哪？
===========================================
 - 每次执行完成后，会自动打开日志目录（如果不希望自动打开，可以注释掉脚本最后一段中Open命令开头的代码）
 - 日志路径：icloud云盘 -> LOG -> 当前计算机名称 ->
 - 启动台LaunchPad的备份路径：icloud云盘 -> BACKUP -> 当前计算机名称 -> 当前用户名称 -> DesktopLayout



===========================================
如何设置定时更新？
===========================================
 - 建议使用 Lingon Pro（或类似软件），将usercmd_updatefull挂载为循环任务（比如每天夜间3点钟执行）
    https://lingon-x.macupdate.com/

 - 挂载命令：
    + 方法 1 : /usr/bin/open /usr/local/bin/usercmd_updatefull                                （推荐，前台执行）
    + 方法 2 : /opt/homebrew/bin/bash /usr/local/bin/usercmd_updatefull                       （后台执行）
    + 方法 3 ：/usr/bin/sudo -n /opt/homebrew/bin/bash /usr/local/bin/usercmd_updatefull      （后台执行 + 以ROOT权限执行）

    PS：
    如果命令被存储到 /usr/local/bin 路径下 ！



===========================================
出现Error的应对
===========================================

如果出现如下错误：

/opt/homebrew/etc/bash_completion.d/ipfs: line 5526: complete: nosort: invalid option name
/opt/homebrew/etc/bash_completion.d/mycli: line 25: complete: nosort: invalid option name
/opt/homebrew/etc/bash_completion.d/wg: line 99: complete: nosort: invalid option name
/opt/homebrew/etc/bash_completion.d/wg-quick: line 43: complete: nosort: invalid option name

则执行此脚本解决问题：

uesrcmd_fix_nosort

在执行后，使用如下命令进行验证：   

grep -R "nosort" /opt/homebrew/etc/bash_completion.d



===========================================
如何避免密码请求？
===========================================


 - 可以通过类似于（  brew upgrade --debug --verbose --greedy ）命令，查看是哪些程序触发了密码请求，
 - 然后将对应的命令行，加入到sudo visudo（或直接删除对应的无用程序）

傻瓜配置如下：（注意，用户名，不区分 大小写）
    可通过批量替换功能，将“你的用户名” 替换成 你登录的实际用户名/



# ... 代码开始 ！！！

 Defaults        env_keep += "PATH"
 Defaults        env_keep += "HOME"



# ----------------------------
# 免密更新 MacOS Update
# ----------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/softwareupdate* 



# ----------------------------
# 免密更新 MacPorts
# ----------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port selfupdate*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port sync*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port clean*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port outdated*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port rev-upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port installed inactive*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port uninstall inactive*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -u selfupdate*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -u sync*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -u upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -u clean*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -u outdated*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -u rev-upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -u installed inactive*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -u uninstall inactive*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -N reclaim*



# --------------------------------
# 免密更新 Homebrew 
# --------------------------------

# SETENV: 这个标签用于告诉 sudo 允许使用 -E（preserve environment）等与环境变量相关的功能。
# 如果不加，则不能使用 -E参数！！

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/bin/brew upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/local/bin/brew upgrade*



# -------------------------------
# 免密更新 Homebrew 间接调用
# -------------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/xargs -0 -- /bin/rm --
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/xargs -0 -- /bin/rm -r -f --
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/xargs -0 -- /opt/homebrew/Library/Homebrew/*

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/touch /Applications/*/.homebrew-write-test
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm /Applications/*/.homebrew-write-test

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -R -f -- /Applications/*/Contents
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -R -f -- /Applications/*/*.app

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Applications/*/Contents
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Applications/*/.DS_Store
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Applications/*/Icon*

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm /Applications/*.app/.homebrew-write-test

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/cp -pR /opt/homebrew/Caskroom/*.app/Contents /Applications/*.app

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/chown -R -- 你的用户名\:staff /usr/local/include /usr/local/lib
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/chown -R -- 你的用户名\:staff /Applications/*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/chown -R -- 你的用户名 /usr/local/include /usr/local/lib
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/chown -R -- 你的用户名 /Applications/*

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/swift -target arm64-apple-macosx15 /opt/homebrew/Library/Homebrew/cask/utils/copy-xattrs.swift /opt/homebrew/Caskroom/*.app /Applications/*.app

# 你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/launchctl remove*
# 你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/launchctl list*
# 你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/launchctl unload*
# 你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove*
# 你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list*
# 你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl unload*
# 你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/*
# 你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/*
# 你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil *
# 你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/*


# ----------------------------
# 免密更新 115 浏览器
# ----------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm /Applications/115Browser.app/*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -R -f -- /Applications/115Browser.app/*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/cp -pR /opt/homebrew/Caskroom/115browser/*/115Browser.app/Contents /Applications/115Browser.app
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/swift -target arm64-apple-macosx15 /opt/homebrew/Library/Homebrew/cask/utils/copy-xattrs.swift /opt/homebrew/Caskroom/115browser/*/115Browser.app /Applications/115Browser.app



# ----------------------------
# 免密更新 86box
# ----------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/cp -pR /opt/homebrew/Caskroom/86box*/86Box.app /Applications/86Box/86Box.app


# ------------------------------
# 免密更新 Adobe Creative Cloud 
# ------------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/chown -R -- 你的用户名\:staff /Library/Application\ Support/Adobe
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list Adobe_Genuine_Software_Integrity_Service
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove Adobe_Genuine_Software_Integrity_Service
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.adobe.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.adobe.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.adobe.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.adobe.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/xargs -0 -- /bin/rm -r -f  --
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/Caskroom/adobe-creative-cloud/*/Install.app/Contents/MacOS/Install**


# ---------------------------
# 免密更新 Adguard
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.adguard*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.adguard*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.adguard.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.adguard.mac.adguard*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/adguard/*/AdGuard.pkg *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rmdir -- /Library/Application\ Support/AdGuard*/com.adguard.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rmdir -- /Library/Application\ Support/AdGuard\ Software


# ---------------------------
# 免密更新 Adobe Acrobat Pro
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list  com.adobe.ARMDC.* 
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.adobe.ARMDC.* 
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.adobe.acrobat.*.pkg*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.adobe.armdc.app.pkg 
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/adobe-acrobat-pro/*/Acrobat/Acrobat\ DC\ Installer.pkg*


# ---------------------------
# 免密更新 AirParrot
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/kext* * /Library/Extensions/AirParrotDriver.kext
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/kext* * /Library/Extensions/APExtFramebuffer.kext
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/kext* * /System/Library/Extensions/AirParrotDriver.kext
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/kext* * /System/Library/Extensions/APExtFramebuffer.kext
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/kext* * com.squirrels.driver.AirParrotSpeakers


# ---------------------------
# 免密更新 Aldente
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.apphousekitchen*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.apphousekitchen*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.apphousekitchen.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.apphousekitchen.*.plist


# ---------------------------
# 免密更新 AltServer
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/cp -pR /Applications/AltServer.app /opt/homebrew/Caskroom/altserver/*/AltServer.app


# ---------------------------
# 免密更新 Anaconda
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/Caskroom/anaconda/*/Anaconda*.sh -b -p /opt/homebrew/anaconda*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/chown -R -- 你的用户名\:staff /opt/homebrew/anaconda*


# ---------------------------
# 免密更新 AppCleaner
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list net.freemacsoft.AppCleaner*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove net.freemacsoft.AppCleaner*


# ---------------------------
# 免密更新 Background Music
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.bearisdriving*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.bearisdriving*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.bearisdriving.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.bearisdriving.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/killall coreaudiod
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.bearisdriving.BGM
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/background-music/*/BackgroundMusic*.pkg *


# ---------------------------
# 免密更新 BasicTeX
# ---------------------------
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list org.tug.mactex.basictex*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove org.tug.mactex.basictex*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/org.tug.mactex.basictex*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/org.tug.mactex.basictex*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget org.tug.mactex.basictex*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/basictex/*/mactex-basictex-*.pkg *


# ---------------------------
# 免密更新 BlueHarvest
# --------------------------- 

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.zeroonetwenty.BlueHarvestHelper*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.zeroonetwenty.BlueHarvestHelper*


# --------------------------- 
# 免密更新 BusyCal
# --------------------------- 

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list *com.busymac.busycal*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove *com.busymac.busycal*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.busymac.busycal*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.busymac.busycal*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.busymac.busycal*.pkg
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/busycal/*/BusyCal*.pkg *


# --------------------------- 
# 免密更新 BusyContacts
# --------------------------- 

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/busycontacts/*/BusyContacts\ Installer.pkg *


# ---------------------------
# 免密更新 Capcut
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/touch /Applications/CapCut*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm /Applications/CapCut*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/cp -pR /opt/homebrew/Caskroom/capcut*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/swift * /opt/homebrew/Library/Homebrew/cask/utils/copy-xattrs.swift /opt/homebrew/Caskroom/capcut*


# ---------------------------
# 免密更新 Cloudflare Warp
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.cloudflare*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.cloudflare*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.cloudflare.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.cloudflare.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /Applications/Cloudflare\ WARP.app/Contents/Resources/uninstall.sh
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/cloudflare-warp/*/Cloudflare_WARP*.pkg *


# ---------------------------
# 免密更新 DaisyDisk
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.daisydiskapp.DaisyDiskAdminHelper
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remvoe com.daisydiskapp.DaisyDiskAdminHelper


# ---------------------------
# 免密更新 Disk Drill
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/Caskroom/disk-drill/*/Disk\ Drill.app/Contents/Resources/uninstall


# ---------------------------
# 免密更新 Dropshare
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/chown -R -- 你的用户名 /Applications/Dropshare*


# ---------------------------
# 免密更新 Dolphin
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/cp -pR /Applications/Dolphin.app /opt/homebrew/Caskroom/dolphin/*/Dolphin.app
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/chown -R -- 你的用户名 /opt/homebrew/Caskroom/dolphin/*/Dolphin.app/Dolphin.app


# ---------------------------
# 免密更新 Eagle
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/eagle/*/Autodesk_EAGLE*.pkg *


# ---------------------------
# 免密更新 Elgato Stream Deck
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.elgato.StreamDeck*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.elgato.StreamDeck*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.elgato.StreamDeck*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/elgato-stream-deck/*/Stream_Deck*.pkg *


# ---------------------------
# 免密更新 ForkLift
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.binarynights.ForkLift*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.binarynights.ForkLift*


# ---------------------------
# 免密更新 Garmin Basecamp 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/garmin-basecamp/*/Install\ BaseCamp.pkg *


# ---------------------------
# 免密更新 gifox
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.gifox.*.agent
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.gifox.*.agent


# ---------------------------
# 免密更新 GOG Galaxy
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.gog.galaxy*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.gog.galaxy*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.gog.galaxy.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.gog.galaxy.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/gog-galaxy/*/galaxy_client*.pkg *


# ---------------------------
# 免密更新 Google Drive
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.google.drivefs*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/google-drive/*/GoogleDrive.pkg *


# ---------------------------
# 免密更新 Google Earth Pro
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.Google.GoogleEarthPro
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/google-earth-pro/*/Install\ Google\ Earth\ Pro*.pkg *


# ---------------------------
# 免密更新GPG Suite
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list org.gpgtools.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove org.gpgtools.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/org.gpgtools.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/org.gpgtools.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/Caskroom/gpg-suite/*/Uninstall.app/Contents/Resources/GPG\ Suite\ Uninstaller.app/Contents/Resources/uninstall.sh
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/gpg-suite/*/Install.pkg *


# ---------------------------
# 免密更新 Hazeover
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.pointum.hazeover.launcher


# ---------------------------
# 免密更新 Hookmark
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.cogsciapps.hookautolaunchhelper


# ---------------------------
# 免密更新 Karabiner Elements
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /Library/Application\ Support/org.pqrs/Karabiner-DriverKit-VirtualHIDDevice/scripts/uninstall/remove_files.sh
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list org.pqrs.karabiner.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /Library/Application\ Support/org.pqrs/Karabiner-Elements/uninstall_core.sh
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/karabiner-elements/*/Karabiner-Elements.pkg *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget org.pqrs.Karabiner*


# ---------------------------
# 免密更新 Linkliar
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list io.github.halo.linkdaemon
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list io.github.halo.linkhelper


# ---------------------------
# 免密更新 Loop
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Applications/Loop.app/*


# ---------------------------
# 免密更新 Logitech : Logi  Options+ \ Logi Options
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.logi.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.logi.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/Caskroom/logi-*/*/logi*_installer.app/Contents/MacOS/logi*_installer *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.logi.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.logi.*.plist


# ---------------------------
# 免密更新 MacCleaner Pro
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/touch /Applications/MacCleaner*/.homebrew-write-test
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Applications/MacCleaner*/.DS_Store
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Applications/MacCleaner*/Icon*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -R -f -- /Applications/MacCleaner*/MacCleaner*.app
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/cp -pR /opt/homebrew/Caskroom/maccleaner-pro/*/MacCleaner*/* /Applications/MacCleaner*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/touch /Applications/MacCleaner*/.homebrew-write-test
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/rm /Applications/MacCleaner*/.homebrew-write-test
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/cp -pR /opt/homebrew/Caskroom/maccleaner-pro/*/MacCleaner*/* /Applications/MacCleaner*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/swift -target arm64-apple-macosx15 /opt/homebrew/Library/Homebrew/cask/utils/copy-xattrs.swift /opt/homebrew/Caskroom/maccleaner-pro/*/MacCleaner* /Applications/MacCleaner*


# ---------------------------
# 免密更新 Macforge
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.macenhance.MacForge*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.macenhance.MacForge*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.macenhance.MacForge.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.macenhance.MacForge.*.plist


# ---------------------------
# 免密更新 Macfuse
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget io.macfuse.installer*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/macfuse/*/Extras/macFUSE*.pkg *


# ---------------------------
# 免密更新 MacUpdater
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.corecode.MacUpdater*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.corecode.MacUpdater*


# ---------------------------
# 免密更新 Micro Snitch
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list at.obdev.MicroSnitch*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove at.obdev.MicroSnitch*


# -------------------------------
# 免密更新 Microsoft Auto Update
# -------------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.microsoft.autoupdate*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.microsoft.update*i
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.microsoft.autoupdate*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.microsoft.update*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.microsoft.autoupdate.helper.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.microsoft.update.agent.plist 
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.microsoft.package.Microsoft_AutoUpdate.app
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-auto-update/*/Microsoft_AutoUpdate_*.pkg *


# ---------------------------
# 免密更新 Microsoft Edge
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.microsoft.EdgeUpdater*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.microsoft.EdgeUpdater*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.microsoft.edgemac
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-edge/*/MicrosoftEdge*.pkg *


# ---------------------------------------------------------------------------------
# 免密更新 Microsoft Office : Ecxel Word PowerPoint Outlook OneNote OneDrive Visio
# ---------------------------------------------------------------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.microsoft.office.licensing*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.microsoft.office.licensing*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.microsoft.office.licensing*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.microsoft.office.licensing*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.microsoft.package.Microsoft_*.app
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.microsoft.pkg.licensing
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-excel/*/Microsoft_Excel_*.pkg *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-word/*/Microsoft_Word_*.pkg *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-powerpoint/*/Microsoft_PowerPoint_*.pkg *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-outlook/*/Microsoft_Outlook_*.pkg *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-onenote/*/Microsoft_OneNote_*.pkg *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-onedrive/*/Microsoft_OneDrive_*.pkg *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-visio/*/Microsoft_Visio_*.pkg *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-teams/*/Microsoft_Teams_*.pkg *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-edge/*/Microsoft_Edge_*.pkg *


# ---------------------------
# 免密更新 Microsoft Teams
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.microsoft.teams*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remvoe com.microsoft.teams*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-teams/*/MicrosoftTeams*.pkg *


# ---------------------------
# 免密更新 Microsoft OneDrive
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.microsoft.OneDrive*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.microsoft.OneDrive*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.microsoft.OneDrive*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.microsoft.OneDrive*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.microsoft.SyncReporter
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.microsoft.SyncReporter
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.microsoft.SyncReporter.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.microsoft.SyncReporter.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.microsoft.OneDrive
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/onedrive/*/OneDrive.pkg *


# -------------------------------
# 免密更新 Microsofti Windows App
# -------------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.microsoft.update*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.microsoft*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/windows-app/*/Windows_App_*.pkg *


# ---------------------------
# 免密更新 MiPony
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget net.installer.mipony*.pkg
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/mipony/*/Mipony-Installer.pkg *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/microsoft-onedrive/*/MicrosoftOneDrive*.pkg *


# ---------------------------
# 免密更新 Mist 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.ninxsoft.mist*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.ninxsoft.mist*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.ninxsoft.mist*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.ninxsoft.mist*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.ninxsoft.pkg.mist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/mist/*/Mist.*.pkg *


# ------------------------------------
# 免密更新 Mono MDK for Visual Studio
# ------------------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rmdir -- /Library/Frameworks/Mono.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.xamarin.*.pkg
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/mono-mdk-for-visual-studio/*/MonoFramework-MDK-*.pkg *


# ---------------------------
# 免密更新 NextCloud 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.nextcloud*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.nextcloud*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.nextcloud.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/nextcloud/*/Nextcloud*.pkg *


# ---------------------------
# 免密更新 nperf
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/nperf/*/nPerf-*.pkg *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.nperf*


# ---------------------------
# 免密更新 numi
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.dmitrynikolaev*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.dmitrynikolaev*


# ---------------------------
# 免密更新 nVidia GeForce Now
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/chmod -- u+rw /Applications/NVIDIA\ GeForce\ NOW.app *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/xattr -w * /Applications/NVIDIA\ GeForce\ NOW.app


# ---------------------------
# 免密更新 OpenVPN
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list org.openvpn*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove org.openvpn*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/org.openvpn.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/org.openvpn.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget org.openvpn.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/openvpn-connect/*.pkg *


# ---------------------------
# 免密更新 OverSight
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/Caskroom/oversight/*/OverSight*.app/Contents/MacOS/OverSight*


# --------------------------------------
# 免密更新 Paragon ExtFS \ Paragon NTFS
# --------------------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list application.com.paragon-software.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.paragon-software.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove application.com.paragon-software.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.paragon-software.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /sbin/kextunload * com.paragon-software.filesystems.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/kextstat * com.paragon-software.filesystems.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/kextfind * com.paragon-software.filesystems.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -rf /Library/Extensions/ufsd_*.kext
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.paragon-software.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.paragon-software.*.plist


# ---------------------------
# 免密更新 Parsec 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget tv.parsec.www
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/parsec/*.pkg *
 

# ---------------------------
# 免密更新 PowerShell 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/powershell/*.pkg *


# ---------------------------
# 免密更新 Pritunl
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.pritunl*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.pritunl*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.pritunl.service.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.pritunl.service.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.pritunl.pkg.Pritunl
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/pritunl/*.pkg * 


# ---------------------------
# 免密更新 Sensei
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list org.cindori.Sensei*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove org.cindori.Sensei*


# ---------------------------
# 免密更新 Stash
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list ws.stash.app.mac.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove ws.stash.app.mac.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/PrivilegedHelperTools/ws.stash.app.mac.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl load -w /Library/LaunchDaemons/ws.stash.app.mac.*


# ---------------------------
# 免密更新 SourceTree
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.atlassian.SourceTree*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.atlassian.SourceTree*


# ---------------------------
# 免密更新 Spotify 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.spotify*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.spotify*


# --------------------------- 
# 免密更新 Steam
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.valvesoftware.steam*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.valvesoftware.steam*


# ---------------------------
# 免密更新 Synology Drive
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.synology.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.synology.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/synology*/*/*Synology*.pkg *


# ---------------------------
# 免密更新 Tailscale
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.tailscale*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/tailscale*/*.pkg *


# ---------------------------
# 免密更新 TextSniper 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.valerijs.boguckis.gumroad.TextSniper*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.valerijs.boguckis.gumroad.TextSniper*


# ---------------------------
# 免密更新 TeX
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /Library/TeX/texbin/tlmgr update*


# ---------------------------
# 免密更新 TimemaChineeEditor
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.tclementdev.timemachineeditor*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.tclementdev.timemachineeditor*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.tclementdev.timemachineeditor*.plist 
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.tclementdev.timemachineeditor*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.tclementdev.pkg.timemachineeditor
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/timemachineeditor/*.pkg *


# ---------------------------
# 免密更新 Trim Enabler 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list org.cindori*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove org.cindori*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/org.cindori.TE*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/org.cindori.TE*.plist


# ---------------------------
# 免密更新 TripMode
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list ch.tripmode.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove ch.tripmode.*


# ---------------------------
# 免密更新 TunnelBlick
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list net.tunnelblick.tunnelblick*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove net.tunnelblick.tunnelblick*


# ---------------------------
# 免密更新 UninstallPKG
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.corecode.UninstallPKG*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.corecode.UninstallPKG* 
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.corecode.UninstallPKG*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.corecode.UninstallPKG*.plist


# ---------------------------
# 免密更新 VeraCrypt 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.idrix.pkg.veracrypt
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/veracrypt/*.pkg *


# ---------------------------
# 免密更新 Visual Studio Code 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.microsoft.VSCode*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.microsoft.VSCode*


# ---------------------------
# 免密更新 WhatRoute 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list net.whatroute*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove net.whatroute*


# ---------------------------
# 免密更新 WIFI Explorer
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.intuitibits.wifiexplorer*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.intuitibits.wifiexplorer*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.intuitibits.wifiexplorer*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.intuitibits.wifiexplorer*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget com.intuitibits.wifiexplorer*.pkg
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/wifi-explorer-pro/*.pkg *


# ---------------------------
# 免密更新 WireShark 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list org.wireshark*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove org.wireshark*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/org.wireshark*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/org.wireshark*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget org.wireshark*.pkg
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/wireshark*/*.pkg *


# ---------------------------
# 免密更新 Xquartz 
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list org.xquartz*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove org.xquartz*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/org.xquartz.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/org.xquartz.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget org.xquartz.X11
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/xquartz/*.pkg *


# ---------------------------
# 免密更新 ZeroTier
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list com.zerotier.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove com.zerotier.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/com.zerotier.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/com.zerotier.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/zerotier*/*/ZeroTier*.pkg *


# ---------------------------
# 免密更新 Zoom
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl list us.zoom*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/launchctl remove us.zoom*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchDaemons/us.zoom.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f -- /Library/LaunchAgents/us.zoom.*.plist
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/pkgutil --forget us.zoom.*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/installer -pkg /opt/homebrew/Caskroom/zoom/*.pkg *



# ---------------------------
# 免密执行：自动更新脚本
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/local/bin/usercmd_updatefull
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/local/bin/usercmd_updatefull_NonMacUpdater
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/bin/bash /usr/local/bin/usercmd_updatefull



# --------------------------------------------------------
# 系统密码 统缓时间 （缓存实效后，才需要二次输入密码）
# --------------------------------------------------------

Defaults timestamp_timeout=3            #  缓存   3 分钟
# Defaults timestamp_timeout=120        #  缓存 120 分钟 (不建议)






# ... 代码结束 ！！！！！！！





===========================================
如何检测visudo是否生效
===========================================

sudo -l


===========================================
如何查看visudo配置文件
===========================================

sudo cat /etc/sudoers



===========================================
如何编辑visudo配置文件 
===========================================

sudo visudo
注意，退出保存时，新配置回立刻生效，无需手动reload 



===========================================
特别提示： 
===========================================

在编辑visudoers时，一定 一定 一定 一定 不要修改(删除) 如下配置！！！
root            ALL = (ALL) ALL

不然不可能要要面临重装系统！



===========================================
其他： 
===========================================

如果想 全自动 “强制升级” 已安装的 “所有Homebrew cask软件” ，可以使用如下命令：

brew tap buo/cask-upgrade
brew cu -a -y -f


如果想 全自动 “强制重装” 已安装的 “所有Homebrew软件” ，可以使用如下命令：

# 重装所有 命令行软件
brew list --formula | xargs brew reinstall --force
或（遇到Error也不间断执行，错误记录在txt文件当中）
brew list --formula | xargs -I {} sh -c 'brew reinstall --force --debug --verbose {} || echo "Error reinstalling {}" >> reinstall_formula_errors.txt'


# 重装所有 图形软件
brew list --cask    | xargs brew reinstall --force --cask --debug --verbose
或（遇到Error也不间断执行，错误记录在txt文件当中）
brew list --cask | xargs -I {} sh -c 'brew reinstall --force --cask --debug --verbose {} || echo "Error reinstalling {}" >> reinstall_cask_errors.txt'



# 如果发生中断，请使用如下命令继续执行
brew list --formula | sed -n '/软件的homebrew英文名称/,$p' | xargs brew reinstall --force --debug --verbose
brew list --cask    | sed -n '/软件的homebrew英文名称/,$p' | xargs brew reinstall --force --debug --verbose --cask

或（遇到Error也不间断执行，错误记录在txt文件当中）
brew list --cask | sed -n '/软件的homebrew英文名称/,$p' | xargs -I {} sh -c 'brew reinstall --force --debug --verbose --cask {} || echo "Error reinstalling {}" >> reinstall_cask_errors.txt'



# 如果想快速将APP与homebrew解绑，而不删除app，请使用如下命令

rm -rf /opt/homebrew/Caskroom/应用名.app
brew clwanup

如：
rm -rf /opt/homebrew/Caskroom/microsoft-excel
rm -rf /opt/homebrew/Caskroom/microsoft-outlook
rm -rf /opt/homebrew/Caskroom/microsoft-powerpoint
rm -rf /opt/homebrew/Caskroom/microsoft-onenote
rm -rf /opt/homebrew/Caskroom/microsoft-onedrive


===========================================
声明：
===========================================

 - 本脚本，代码均来自AI，人工仅仅做极少量调整维护。参与编程的AI包括：
   1. 【2025/Q3-？】编程者：OpenAI  ChatGPT O3—Pro + 深度研究 （主力）  、  xAI Grok4 （少量）
   2. 【2025/Q1-Q2】编程者：xAI     Grok3 + DeeperSearch / Think
   3. 【2024/Q3-Q4】编程者：OpenAI  ChatGPT O1 
   
 - 本脚本经过长期测试。
 
 - 本工程所有源代码，均使用MIT协议分发
