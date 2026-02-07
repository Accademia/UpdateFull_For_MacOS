
# 功能与目的：

 1. 一键更新 ， Mac OS 系统中的 APP 
 
 2. 每日静默更新 ， Mac OS 系统中的 APP （配合 Lingon Pro 软件）

 3. 更新过程中，无需输入 管理员密码 （配合 Generate_Homebrew_Sudoers 项目）
    
 4. 更新结束后，不打乱 启动台布局（LaunchPad Layout）

<br>

最终：
 - 用户体验 = AppStore 每日夜间 静默更新
 - 覆盖率  >  90% Mac 软件



<br>
<br>

---------


# 可以更新哪些程序？

<br>

本脚本是 聚合更新软件，会调用 所有主流更新软件，更新所有mac软件。
被聚合的 更新软件包括：
  - HomeBrew	
    - 更新 Homebrew 下载的APP
  - Sparkle
    - 更新 Sparkle 下载的APP
  - Mas		
    - 更新 App Store 下载的APP
  - MacPorts	
    - 更新 MacPorts下载的APP（Linux移植）
  - TopGreade	
    - 更新 更多在线安装的APP
  - MacUpdater	
    - 更新 所有APP

在更新后，自动还原LaunchPad（启动台）布局


<br>
<br>


---------

# 最终效果：

<br>

<img width="700"  alt="example" src="https://github.com/user-attachments/assets/eaf6ff98-7180-4420-8f56-ba77471028b1" />




<br>
<br>

---------


# 程序下载

<br>

# 命令 A ：  [usercmd_updatefull_NonMacUpdater](https://cdn.jsdelivr.net/gh/Accademia/UpdateFull_For_MacOS/usercmd_updatefull_NonMacUpdater)  
 
🔥🔥🔥 推荐 🔥🔥🔥

> ### https://cdn.jsdelivr.net/gh/Accademia/UpdateFull_For_MacOS/usercmd_updatefull_NonMacUpdater

<br>

# 命令 B ： [usercmd_updatefull](https://cdn.jsdelivr.net/gh/Accademia/UpdateFull_For_MacOS/usercmd_updatefull)   

❌❌❌ 不推荐 ❌❌❌ ： 因为 MacUpdater 已停止更新 

> ### https://cdn.jsdelivr.net/gh/Accademia/UpdateFull_For_MacOS/usercmd_updatefull


<br>
<br>

---------


# 上述两者区别

<br>

上述两个程序，唯一的区别就是 后者 增加了 ，通过 MacUpdater 更新所有APP的功能。其他方面无任何区别 ！ 但是MacUpdater 已经在 2026-01-01 停止更新了。所以建议下载 [usercmd_updatefull_NonMacUpdater](https://cdn.jsdelivr.net/gh/Accademia/UpdateFull_For_MacOS/usercmd_updatefull_NonMacUpdater) 程序

<br>
<br>

---------

# 使用方法（纯鼠标）

<br>

 1. 下载 [usercmd_updatefull](https://cdn.jsdelivr.net/gh/Accademia/UpdateFull_For_MacOS/usercmd_updatefull)  或 [usercmd_updatefull_NonMacUpdater](https://cdn.jsdelivr.net/gh/Accademia/UpdateFull_For_MacOS/usercmd_updatefull_NonMacUpdater)  (无需下载其他文件)
 
 2. 然后 双击执行

<br>
<br>

---------

# 使用方法（命令行）

<br>

 1.  下载 usercmd_updatefull_NonMacUpdater (无需下载其他文件)
 
 2.  请将命令拷贝至：/usr/local/bin 目录中（或将文件所在的路径，加入到环境变量的$PATH中）
 
 3.  使用 usercmd_updatefull_NonMacUpdater 命令，启动脚本
 
 4.  如果要每日自动执行，建议使用 Lingon，将 usercmd_updatefull_NonMacUpdater 挂载为循环任务（比如每天夜间3点钟执行）
   
   - 挂载命令 ：
      ```
      /usr/bin/sudo -n -E /opt/homebrew/bin/bash /usr/local/bin/usercmd_updatefull_NonMacUpdater 
      ```

注意:

  1.  本脚本会检测，更新所需的服务器，是否能正常访问，如果不能正常访问，建议开启VPN。

  2.  本脚本，可配置 在 几点几分 到 几点几分 之间，拒绝执行（以避免和其他定时更新的软件冲突）



<br>
<br>

---------


# 日志保存在哪？

<br>

 - 每次执行完成后，会自动打开日志目录

 - 日志路径：
   - icloud云盘 -> LOG -> 产品名-芯片型号-序列号 ->

 - 启动台LaunchPad的备份路径：
   - icloud云盘 -> BACKUP -> 产品名-芯片型号-序列号 -> 当前用户名称 -> DesktopLayout

注意：

  - 没有使用计算机名称：计算机名称会因为网络冲突，而变化，会导致log目录和备份目录频繁改变，而且无法关闭自动改名，这是macos系统功能之一。所以不能使用计算机名称。
 
  - 另外，将LOG日志写入iCLOUD，也便于异地检测更新状态。用iPhone都可以查看iCloud中其他Mac的有没有按时更新，以及以往每次、每天的更新记录。


<br>
<br>

---------

# 可管理的应用太少，怎么办？

<br>

 - 我发现，通过本脚本升级的APP数量太少，只有总APP数量的1/3 ，怎么办 ？ 请看如下：

 - 只要软件尽量通过homebrew和appstore这两个渠道去安装，哪怕在没有MacUpdater的参与下，更新覆盖率也能接近90%。请看，下述案例：
 
 - 以一台800多个软件的Mac为例：
     - Homebrew Cask ≈ 450
     - Mac AppStore  ≈ 250
     - Sparkle       ≈ 200（与homebrew去重后 ≈ 40）
     - MacUpdater    ≈ 800（与前三者去重后 ≈ 80）

 - 所以，核心 = 如何将APP迁移到 homebrew 、 Mac AppStpre

 - 最终，只要解决一个问题：
   ✅ 如何将 本地已经安装的APP，自动化的，将APP迁移到homebrew ？ 如下：
    迁移命令（可在本项目中下载此命令）：
    ~~~
    ./usercmd_migrate_macapp_to_homebrew
    ~~~
    
 -  上述命令执行后，会生成如下文件
    ~~~
    ./brew_install_commands.txt	# ✅ ✅ ✅ 此文件为 可以迁移的软件列表
    ./non_brew_non_mas_apps.txt	
    ./ambiguous_matches.txt
    ~~~

    brew_install_commands.txt 内含了，迁移所需的全部命令。注意，迁移过程会触发多次密码请求。



<br>
<br>

---------

# 如何设置定时更新？

<br>

 - 如何实现，类似AppStore一样，每天夜间，自动静默更新 所有 Mac APP ？ 请看如下：
 - 建议使用 Lingon Pro（或类似软件），将usercmd_updatefull挂载为循环任务（比如每天夜间3点钟执行）
    - https://lingon-x.macupdate.com/

 - (后台执行) 挂载命令 ：
    + 方法 1 : 打开窗口执行 （相当于 前台执行）
        ```
        /usr/bin/open /usr/local/bin/usercmd_updatefull_NonMacUpdater                                
        ```
    
    + 方法 2 : 静默执行 （相当于 后台执行）
        ```
        /opt/homebrew/bin/bash /usr/local/bin/usercmd_updatefull_NonMacUpdater                       
        ```
     
    + 方法 3 : 静默执行 （相当于 模拟前台终端 的 后台执行）
        ```
        /usr/bin/script -q /dev/null /bin/zsh -lc "/usr/local/bin/usercmd_updatefull_NonMacUpdater"
        ```
     
    + 方法 4 ：以Roo权限 静默执行 （相当于 root 后台执行）
        ```
        /usr/bin/sudo -n -E /opt/homebrew/bin/bash /usr/local/bin/usercmd_updatefull_NonMacUpdater      
        ```
     
    + 特别注意：⚠️⚠️⚠️⚠️  
        + 后台执行usercmd_updatefull时，千万不要不加 /opt/homebrew/bin/bash ，而直接调用 /usr/local/bin/usercmd_updatefull_NonMacUpdater 。这会导致Generate_Homebrew_Sudoers生成的免密规则失效。
        + 因为 程序Generate_Homebrew_Sudoers在前台生成免密规则，不能足量覆盖/usr/local/bin/usercmd_updatefull_NonMacUpdater 程序在后台 直接执行时的所有免密请求，会多出来特别多 在前台生成规则时 无法看到的高权限命令。从而导致执行被卡住。
        + 但是前台调用/usr/local/bin/usercmd_updatefull_NonMacUpdater 时，无上述限制！！

    - PS：
      - 建议命令被存储到 /usr/local/bin 路径下 ！



<br>
<br>

---------

# 如何避免密码请求？

<br>

 - 如何让每次执行本脚本 更新APP (包括定时更新) 的时候，都不弹出管理员密码请求，全程能全自动更新 ？ 请看如下：
 - 需要通过 sudo visudo，将要程序加入到免密列表当中

傻瓜配置如下：（注意，用户名，不区分 大小写）
    可通过批量替换功能，将“你的用户名” 替换成 你登录的实际用户名/

```

# ... 代码开始 ！！！

 Defaults        env_keep += "PATH"
 Defaults        env_keep += "HOME"


# ----------------------------
# 免密执行 本脚本
# ----------------------------

你的用户名 ALL=(root) NOPASSWD: SETENV: /usr/local/bin/usercmd_updatefull
你的用户名 ALL=(root) NOPASSWD: SETENV: /usr/local/bin/usercmd_updatefull_NonMacUpdater

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
# 免密更新 MAS
# ----------------------------

你的用户名 ALL=(root) NOPASSWD: /usr/bin/killall installd


# ----------------------------
# 免密更新 MacPorts
# ----------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port selfupdate*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port sync*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port clean*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port outdated*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port rev-upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port uninstall*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port reclaim*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port echo leaves*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* selfupdate*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* sync*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* clean*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* outdated*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* rev-upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* uninstall*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* reclaim*

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f /tmp/macports*.pkg
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/local/bin/port -* reclaim
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/touch /tmp/.com.apple.dt.CommandLineTools.installondemand.in-progress
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /bin/rm -f /tmp/.com.apple.dt.CommandLineTools.installondemand.in-progress



# --------------------------------
# 免密更新 Homebrew 
# --------------------------------

# SETENV: 这个标签用于告诉 sudo 允许使用 -E（preserve environment）等与环境变量相关的功能。
# 如果不加，则不能使用 -E参数！！

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/bin/brew upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/local/bin/brew upgrade*


# --------------------------------
# 免密更新 MAS 
# --------------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/bin/mas upgrade*
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/bin/mas update*


# --------------------------------
# 免密更新 其他模块 
# --------------------------------
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /Library/TeX/texbin/tlmgr update *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /opt/homebrew/opt/ruby/bin/gem update *


# ---------------------------
# 免密修改：计算机名称
# ---------------------------

你的用户名 ALL=(root) NOPASSWD: /usr/sbin/scutil --set ComputerName *
你的用户名 ALL=(root) NOPASSWD: /usr/sbin/scutil --set LocalHostName *


# ---------------------------
# 免密执行：索引重建
# ---------------------------

你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/mdutil -E *
你的用户名 ALL=(ALL) NOPASSWD: SETENV: /usr/bin/mdutil -i on *


# -------------------------------
# 免密更新 Homebrew Reinstall、Upgrade、Install 调用
# -------------------------------

# 建议通过 generate_homebrew_sudoers.py 自动生成这部分配置规则 （因为平均每个软件有300条规则）
# 请看 Github项目： Generate_Homebrew_Sudoers （自动生成Homebrew APP升级所需的visudo免密配置）



```

- ⚠️⚠️⚠️全自动方式： 建议通过 generate_homebrew_sudoers.py 自动生成，更多免密配置规则（注意，生成的配置与上述配置不重叠，两者都要填入sudoers）
   - ✅ ✅ ✅ 项目：https://github.com/Accademia/Generate_Homebrew_Sudoers
   
- 手动方式：你也可以通过如下命令，手动查看是哪些程序触发了密码请求。然后将对应的命令行，加入到visudo
     ```
     brew upgrade --debug --verbose --greedy
     ```

- 如何编辑visudo配置文件 

   ```
   sudo visudo
   ```


- ⚠️⚠️ 在编辑visudoers时，一定 一定 一定 一定 不要修改(删除) 如下配置！！！ 不然不可能要要面临重装系统！

   ```
   root            ALL = (ALL) ALL
   ```


<br>
<br>

---------

   
#  HomeBrew更新后，须 “手动更新” 的软件

<br>

   ```
   paragon-extfs
   paragon-ntfs
   ```


<br>
<br>

---------


# 出现Error的应对


<br>

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

```
grep -R "nosort" /opt/homebrew/etc/bash_completion.d
```


<br>
<br>

---------


# Mac App Store 更新错误怎么办 ？

<br>

对于MAS应用更新 出错的情况。往往是因为，MacOS系统更新后，苹果修改了MAS升级接口导致的。由于mas-cli逆向出来的苹果接口，不是公开的接口。所以跟进修改需要大量开发时间。

你能做的就是，等待mac-cli作者更新。而本脚本 ，在每次升级前，会自动拉取 mas-cli 最新版本的更新。无需你手工干预。

也就是说，你什么都不做。坐等mas-cli开发者修复，就可以了.


<br>
<br>

---------


# 声明：

<br>

 - 本脚本，代码均来自AI，人工仅仅做极少量调整维护。参与编程的AI包括：
   
   1. 【2025/Q4-？】编程者：OpenAI  ChatGPT 5-Pro + Agent + CodeX
   
   2. 【2025/Q3-Q3】编程者：OpenAI  ChatGPT O3-Pro + 深度研究 （主力）  、  xAI Grok4 （少量）
   
   3. 【2025/Q1-Q2】编程者：xAI     Grok3 + DeeperSearch / Think
   
   4. 【2024/Q3-Q4】编程者：OpenAI  ChatGPT O1 
   
 - 本脚本经过长期测试。
 
 - 本工程所有源代码，均使用MIT协议分发
