
功能与目的：
--------------------------------
 一键更新 MacOS 内安装的所有 第三方软件


.


最终效果：
--------------------------------

<img width="700"  alt="example" src="https://github.com/user-attachments/assets/eaf6ff98-7180-4420-8f56-ba77471028b1" />



.


可以更新哪些程序？
--------------------------------
 - 本脚本会，调用以下个程序，完成自动化更新
	- HomeBrew	：更新 Homebrew下载的APP
	- Mas		：更新 App Store下载的APP
	- MacPorts	：更新 MacPorts下载（Linux移植）的APP
	- TopGreade	：更新 复查上述在线安装的APP
	- MacUpdater	：更新 下载后离线安装的APP
在更新后，自动还原LaunchPad（启动台）布局

.

使用方法：
--------------------------------
1. 下载usercmd_updatefull
2. 在桌面 创建 “快捷方式”（点击后调用本脚本）
3，请将命令拷贝至：/usr/local/bin目录中（或将文件所在的路径，加入到环境变量的$PATH中）
4. 使用usercmd_updatefull命令，启动脚本
5. 如果要每日自动执行，建议使用 Lingon X（或类似软件），将usercmd_updatefull挂载为循环任务（比如每天夜间3点钟执行）

注意:
1. 本脚本会检测，目标网站是否能正常访问，如果不能正常访问，建议开启VPN，并将目标网站挂载到黑名单中。
2. 本脚本，在凌晨 2:50 - 3:00之间，拒绝执行（因为：假定 本脚本会在3:00自动执行）


.


日志保存在哪？
--------------------------------
 - 每次执行完成后，会自动打开日志目录（如果不希望自动打开，可以注释掉脚本最后一段中Open命令开头的代码）
 - 日志路径：icloud云盘 -> LOG -> 产品名-芯片型号-序列号 ->
 - 启动台LaunchPad的备份路径：icloud云盘 -> BACKUP -> 产品名-芯片型号-序列号 -> 当前用户名称 -> DesktopLayout

注意：没有使用计算机名称：计算机名称会因为网络冲突，而变化，会导致log目录和备份目录频繁改变，而且无法关闭自动改名，这是macos系统功能之一。所以不能使用计算机名称。

.


如何设置定时更新？
--------------------------------
 - 建议使用 Lingon Pro（或类似软件），将usercmd_updatefull挂载为循环任务（比如每天夜间3点钟执行）
    https://lingon-x.macupdate.com/

 - 挂载命令：
    + 方法 1 : /usr/bin/open /usr/local/bin/usercmd_updatefull                                （推荐，前台执行）
    + 方法 2 : /opt/homebrew/bin/bash /usr/local/bin/usercmd_updatefull                       （后台执行）
    + 方法 3 ：/usr/bin/sudo -n /opt/homebrew/bin/bash /usr/local/bin/usercmd_updatefull      （后台执行 + 以ROOT权限执行）

    PS：
    如果命令被存储到 /usr/local/bin 路径下 ！


.


出现Error的应对
--------------------------------

如果出现如下错误：
```
/opt/homebrew/etc/bash_completion.d/ipfs: line 5526: complete: nosort: invalid option name
/opt/homebrew/etc/bash_completion.d/mycli: line 25: complete: nosort: invalid option name
/opt/homebrew/etc/bash_completion.d/wg: line 99: complete: nosort: invalid option name
/opt/homebrew/etc/bash_completion.d/wg-quick: line 43: complete: nosort: invalid option name
```
则执行此脚本解决问题：
```
uesrcmd_fix_nosort
```
在执行后，使用如下命令进行验证：   

grep -R "nosort" /opt/homebrew/etc/bash_completion.d


.


如何避免密码请求？
--------------------------------


 - 可以通过类似于（  brew upgrade --debug --verbose --greedy ）命令，查看是哪些程序触发了密码请求，
 - 然后将对应的命令行，加入到sudo visudo（或直接删除对应的无用程序）

傻瓜配置如下：（注意，用户名，不区分 大小写）
    可通过批量替换功能，将“你的用户名” 替换成 你登录的实际用户名/

```

# ... 代码开始 ！！！

 Defaults        env_keep += "PATH"
 Defaults        env_keep += "HOME"


# ----------------------------
# 免密备份 控制台（LaunchPad）
# ----------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/cp -rf /System/Volumes/Data/private/var/folders/*/0/com.apple.dock.launchpad * 
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/chmod  -R 755 ./com.apple.dock.launchpad

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/cp -rf */com.apple.dock.launchpad /System/Volumes/Data/private/var/folders/*/0


# ----------------------------
# 免密更新 MacOS Update
# ----------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/softwareupdate* 
#你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/sbin/softwareupdate --install --all --force --verbose*


# ----------------------------
# 免密更新 MacPorts
# ----------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port selfupdate*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port sync*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port clean*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port outdated*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port rev-upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port uninstall inactive*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* selfupdate*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* sync*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* clean*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* outdated*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* rev-upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* uninstall inactive*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* reclaim*


# --------------------------------
# 免密更新 Homebrew 
# --------------------------------

# SETENV: 这个标签用于告诉 sudo 允许使用 -E（preserve environment）等与环境变量相关的功能。
# 如果不加，则不能使用 -E参数！！

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/bin/brew upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/local/bin/brew upgrade*



# -------------------------------
# 免密更新 Homebrew Reinstall、Upgrade、Install 调用
# -------------------------------

# 建议通过 generate_homebrew_sudoers.py 自动生成这部分配置规则 （因为平均每个软件有300条规则）
# 请看 Github项目： Generate_Homebrew_Sudoers （自动生成Homebrew APP升级所需的visudo免密配置）



```

Generate_Homebrew_Sudoers ： https://github.com/Accademia/Generate_Homebrew_Sudoers



.

如何检测visudo是否生效
--------------------------------
```
sudo -l
```

.

如何查看visudo配置文件
--------------------------------
```
sudo cat /etc/sudoers
```

.

如何编辑visudo配置文件 
--------------------------------
```
sudo visudo
```
注意，退出保存时，新配置回立刻生效，无需手动reload 


.

在homebrew中需要手动更新的软件
--------------------------------

请注意：下述软件，虽然可以通过homebrew安装和更新，但是还需要调用图形界面进行手动操作。

paragon-extfs

paragon-ntfs


.


特别提示： 
--------------------------------

在编辑visudoers时，一定 一定 一定 一定 不要修改(删除) 如下配置！！！
```
root            ALL = (ALL) ALL
```
不然不可能要要面临重装系统！


.

其他： 
--------------------------------

如果想 全自动 “强制升级” 已安装的 “所有Homebrew cask软件” ，可以使用如下命令：
```
brew tap buo/cask-upgrade
brew cu -a -y -f
```

如果想 全自动 “强制重装” 已安装的 “所有Homebrew软件” ，可以使用如下命令：
```
# 重装所有 命令行软件
brew list --formula | xargs brew reinstall --force
或（遇到Error也不间断执行，错误记录在txt文件当中）
brew list --formula | xargs -I {} sh -c 'brew reinstall --force --debug --verbose {} || echo "Error reinstalling {}" >> reinstall_formula_errors.txt'

# 重装所有 图形软件
brew list --cask    | xargs brew reinstall --force --cask --debug --verbose
# 或（遇到Error也不间断执行，错误记录在txt文件当中）
brew list --cask | xargs -I {} sh -c 'brew reinstall --force --cask --debug --verbose {} || echo "Error reinstalling {}" >> reinstall_cask_errors.txt'
```


# 如果发生中断，请使用如下命令继续执行
```
brew list --formula | sed -n '/软件的homebrew英文名称/,$p' | xargs brew reinstall --force --debug --verbose
brew list --cask    | sed -n '/软件的homebrew英文名称/,$p' | xargs brew reinstall --force --debug --verbose --cask
```
或（遇到Error也不间断执行，错误记录在txt文件当中）
```
brew list --cask | sed -n '/软件的homebrew英文名称/,$p' | xargs -I {} sh -c 'brew reinstall --force --debug --verbose --cask {} || echo "Error reinstalling {}" >> reinstall_cask_errors.txt'
```


# 如果想快速将APP与homebrew解绑，而不删除app，请使用如下命令
```
rm -rf /opt/homebrew/Caskroom/应用名.app
brew clwanup
```
如：
```
rm -rf /opt/homebrew/Caskroom/microsoft-excel
rm -rf /opt/homebrew/Caskroom/microsoft-outlook
rm -rf /opt/homebrew/Caskroom/microsoft-powerpoint
rm -rf /opt/homebrew/Caskroom/microsoft-onenote
rm -rf /opt/homebrew/Caskroom/microsoft-onedrive
```
.

声明：
--------------------------------

 - 本脚本，代码均来自AI，人工仅仅做极少量调整维护。参与编程的AI包括：
   1. 【2025/Q4-？】编程者：OpenAI  ChatGPT 5-Pro + Agent 
   1. 【2025/Q3-Q3】编程者：OpenAI  ChatGPT O3-Pro + 深度研究 （主力）  、  xAI Grok4 （少量）
   2. 【2025/Q1-Q2】编程者：xAI     Grok3 + DeeperSearch / Think
   3. 【2024/Q3-Q4】编程者：OpenAI  ChatGPT O1 
   
 - 本脚本经过长期测试。
 
 - 本工程所有源代码，均使用MIT协议分发
