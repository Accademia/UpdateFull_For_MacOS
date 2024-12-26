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
 - 本脚本会，直接（或间接）调用以下四个程序，完成自动化更新
	- HomeBrew	：更新 Homebrew
	- Mas		：更新 App Store
	- Port		：更新 Linux移植APP
	- TopGreade	：更新 其他APP
	- MacUpdater	：更新 更多APP


-------------------------------------------
如何设置定时更新？
-------------------------------------------
 - 建议使用 Lingon X（或类似软件），将usercmd_updatefull挂载为循环任务（比如每天夜间3点钟执行）


-------------------------------------------
如何避免密码请求？
-------------------------------------------
 - 可以通过类似于（brew upgrade --cask --debu）命令，查看是哪些程序触发了密码请求，
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
    /opt/homebrew/bin/brew upgrade*, \
    /usr/local/bin/brew upgrade*, \
    /usr/bin/xargs -0 -- /bin/rm -r -f --, \
    /usr/bin/xargs -0 -- /bin/rm --, \
    /opt/homebrew/Library/Homebrew/cask/utils/rmdir.sh

你的用户名 ALL=(ALL) NOPASSWD: SETENV: BREW_UPGRADE_CMDS



-------------------------------------------
声明：
-------------------------------------------
 - 本脚本所有代码均来自于ChatGPT，且经过长期测试。本工程使用MIT协议分发
