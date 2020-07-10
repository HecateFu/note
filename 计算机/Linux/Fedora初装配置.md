# VirtualBox虚拟机屏幕太小
> [在VirtualBox中安装增强功能](https://www.cnblogs.com/ruanjiantest0405/p/10542472.html)
> 
> [在 VirtualBox，VBoxVGA，VMSVGA和VBoxSVGA之间有什么区别？](https://www.kutu66.com//diannao/article_176642)

VirtualBox 的虚拟机屏幕的默认分辨率是 800\*600，虚拟机安装完成后即使在系统中调整了屏幕分辨率，系统重启之后依然会再次变成800\*600。

需要安装 VirtualBox 增强工具(VBoxGuestAdditions)来解决这个问题。

安装步骤：

1. 启动虚拟机 -> 设备 -> 安装增强功能
2. 如果出现“未能加载虚拟光盘 XXX(VBoxGuestAdditions.iso的路径) 到虚拟电脑 XXX”的提示，来 Fedora 系统中用 File 弹出当前加载的光盘，然后再次点击“安装增强功能”
3. 加载成功后会出现一个提示框“VBox_GAs_[版本号]包含打算自动启动的软件。您是否想要运行它？”点击“运行”。然后系统会打开一个命令行窗口运行增强工具的安装程序。
4. 命令行会出现确认安装的提示“Do you wish to continue? [yes or no]”，输入“yes”
5. 安装程序执行完后会提示按 `enter` 关闭安装命令行窗口，然后重启系统。
6. 如果重启系统后虚拟机屏幕依然很小，`视图 -> 虚拟显示屏1` 中的分辨率重设选项都是灰色，且系统中设置的分辨率在重启之后依然失效。检查一下虚拟机的 `设置 -> 显示 -> 显卡控制器`，如果是 `VMSVGA` 关闭虚拟机，把它改成 `VBoxVGA` 或 `VBoxSVGA`。重新启动即可通过 `视图 -> 虚拟显示屏1` 调整分辨率，或者是在系统中调整分辨率重启后依然有效。
7. 此时如果再将“显卡控制器”改为“VMSVGA”增强功能对虚拟机屏幕的优化依然有效。

> 注意：
> 1. 如果“显卡控制器”初始设置是“VBoxVGA”，安装完增强工具重启后就可通过虚拟机或系统内正常设置分辨率了。
> 2. 如果“显卡控制器”初始设置是 **“VBoxSVGA”，不安装增强工具也可以直接调整屏幕分辨率** （通过虚拟机或系统内均可）。是如果换成其他的“显卡控制器”它的设置可能会失效或者无法重设。
> 3. **如果想要使用高分辨率，虚拟机的“显存大小”要设置大一些（我设成64M时不会出现问题）**。默认的是16M，分辨率一调高系统就会死机（无论是通过虚拟机设置还是系统内设置），如果强制关机重启后，如果不将分辨率重新设回800\*600就无法正常显示登陆界面进入系统。
> 
> ***重要总结！！！***
> 
> ***创建虚拟机的时候“显卡控制器”直接选择“VBoxSVGA”，想用高分辨率的图形化界面“显存大小”设高些，增强工具还是装一下比较好。***

- VBoxSVGA: 使用Linux或者 Windows 7或者更高版本的新vm的默认图形控制器。传统VBoxVGA选项相比，这里图形控制器提高了性能和 3D 支持。
    - Linux或者 Windows> 7
    - 提高性能和 3D 支持
- VBoxVGA: 将这里图形控制器用于旧版客户机操作系统。 这是 Windows 7之前 Windows 版本的默认图形控制器。
    - ( 通过推理) 稍微降低性能，但与旧操作系统更兼容
    - 旧操作系统或者 Windows <7
- VMSVGA: 使用这里图形控制器来模拟 VMware SVGA图形设备。
    - 模拟VMWare设备
    - 可能最好是你的VM最初在VMWare上设置并且安装了它们的工具
    - 可能不像VBox卡那样快，因为它试图与其他对象兼容。
- 无: 不模拟图形适配器类型。
    - 你不希望使用图形适配器，即你想要无图或通过SSH运行机器，并且不需要模拟图形的开销。
    - 可以在删除层或者仿真时提高性能

# NAT网络代理设置

(Settings -> Network) or (桌面右上角 status menu -> Wired -> Wired Settings)

-> Network Proxy -> Manual

代理地址填网络的 Default Router 直，端口填真机翻墙软件设置的代理端口，翻墙软件要设置“允许来自局域网的连接”

> [gnome shell](https://en.wikipedia.org/wiki/GNOME_Shell)

# 系统字体设置

> [Changing the font-size?](https://forums.fedoraforum.org/showthread.php?315619-Changing-the-font-size)
>
> [Failed to synchronize cache for repo](https://ask.fedoraproject.org/t/issues-with-dnf-upgrade-on-fedora-30-failed-to-synchronize-cache-for-repo/1142)
> 
> **[Changing Linux GNOME Desktop Fonts Is Easier Than You Think](https://www.makeuseof.com/tag/changing-linux-gnome-desktop-fonts-easier-think/)**

fedora 30 使用 gnome 作为图形界面。系统的 `Settings` 中无法设置系统字体，而 `Displays` 中的 `Scale` 只有 100% 和 200% 两个级别，不像 Win10 中可以拖到设置。1920×1080 分辨率下 Scale 100% 字体太小，200% 显示内容太少。所以还是要调整系统字体才能获得更好的显示 体验。

在gnome中调整系统字体，需要安装 `gnome-tweak-tool` ，执行以下命令安装：
```
sudo dnf install gnome-tweak-tool
```
我在系统安装完成，第一次执行 dnf 命令时遇到了类似下面的错误：
```
Failed to synchronize cache for repo 'fedora'
Error: Failed to synchronize cache for repo 'fedora'
```
执行 `dnf clean all`，然后重新再执行dnf的其他命令需要下载仓库信息时就不会再报上面的错了。

安装完成后，打开 `Tweaks -> Fonts`，`Scaling Factor` 可设置整体缩放，默认值 1 相当于 100%。`Interface Text` 设置 gnome 应用内文字的大小，比如说 File，Settings，Tweaks中文字体。`Monospace Text`设置 Text Editor(gedit)，Terminal 中字体。`Lagency Windows Titles`gnome窗口标题栏字体。`Document Text`文本编辑器？不确定。

我比较喜欢的缩放和字体设置是 `Scaling Factor 1.5`、`Interface Text 12`、`Monospace Text 15`。

# 修改浏览器默认缩放
- Firefox
    - 从设置界面中修改
    
        Preferences -> General -> Zoom
        
    - 直接修改底层属性值
      
        从地址栏访问 `about:config` ，然后搜索 `layout.css.devPixelsPerPx` 属性，默认值是 `-1.0`，1.2代表120%，1.5代表150%
- Chrome
  
    Settings -> Appearance -> Zoom
    
# VirtualBox独占/非独占键盘鼠标

如果 VirtualBox 设置的是非独占键盘时，桌面版系统在使用中会出现 `alt + tab` 快捷键无法切换虚拟机内部的窗口，依然是切换虚拟机窗口和真机的其他窗口的问题。

可以在 `工具 -> 全局设定 -> 热键` 中选择是否“自动独占键盘”。

集成/非集成鼠标设置在虚拟机启动后的窗口的工具栏 `热键 -> 鼠标集成`。但是选择非集成鼠标，在鼠标被虚拟机捕获时，系统内无法正常显示鼠标图标，所以还是用集成鼠标吧。

# 安装Chrome
> 参考资料
> 
> [使用google yum 仓库安装](https://www.if-not-true-then-false.com/2010/install-google-chrome-with-yum-on-fedora-red-hat-rhel/)
> 
> [启用fedora推荐的第三方软件库](https://fedoraproject.org/wiki/Workstation/Third_Party_Software_Repositories)

1. 添加仓库

    添加仓库前已有仓库
    
    ```
    $ dnf repolist
    repo id                        repo name
    fedora                         Fedora 32 - x86_64
    fedora-cisco-openh264          Fedora 32 openh264 (From Cisco) - x86_64
    fedora-modular                 Fedora Modular 32 - x86_64
    updates                        Fedora 32 - x86_64 - Updates
    updates-modular                Fedora Modular 32 - x86_64 - Updates
    $ ls /etc/yum.repos.d/
    fedora-cisco-openh264.repo   fedora-updates.repo
    fedora-modular.repo          fedora-updates-testing-modular.repo
    fedora.repo                  fedora-updates-testing.repo
    fedora-updates-modular.repo
    ```
    
    通过安装 `fedora-workstation-repositories` 来添加仓库，这个软件包含一些仓库文件，安装后可使用 dnf 和gnome-software 查找和安装一些非 fedora 官方库的软件，安装命令如下：
    
    ```
    dnf install fedora-workstation-repositories
    ```
    
    > ***注意：要使用管理员权限执行该命令！***
    > 
    > 使用 `sudo -i` 或 `su -` 进入root账户。
    
    安装后仓库信息如下：
    
    ```
    $ dnf repolist
    repo id                        repo name
    fedora                         Fedora 32 - x86_64
    fedora-cisco-openh264          Fedora 32 openh264 (From Cisco) - x86_64
    fedora-modular                 Fedora Modular 32 - x86_64
    updates                        Fedora 32 - x86_64 - Updates
    updates-modular                Fedora Modular 32 - x86_64 - Updates
    $ ls /etc/yum.repos.d/
    _copr_phracek-PyCharm.repo   fedora-updates-testing-modular.repo
    fedora-cisco-openh264.repo   fedora-updates-testing.repo
    fedora-modular.repo          google-chrome.repo
    fedora.repo                  rpmfusion-nonfree-nvidia-driver.repo
    fedora-updates-modular.repo  rpmfusion-nonfree-steam.repo
    fedora-updates.repo
    ```
    
    repolist中并没有显示新仓库，因为新仓库默认都没有启用，而仓库文件中新增了4个仓库文件，分别支持 PyChram 、Chrome 浏览器、nvidia 驱动、Steam 客户端
    
2. 启用chrome的仓库
   
    执行下面命令启用chrome仓库：
    
    ```
    dnf config-manager --set-enabled google-chrome
    ```
    
    执行完成后repolist中增加了google-chrome：
    
    ```
    $ dnf repolist
    repo id                        repo name
    fedora                         Fedora 32 - x86_64
    fedora-cisco-openh264          Fedora 32 openh264 (From Cisco) - x86_64
    fedora-modular                 Fedora Modular 32 - x86_64
    google-chrome                  google-chrome
    updates                        Fedora 32 - x86_64 - Updates
    updates-modular                Fedora Modular 32 - x86_64 - Updates
    ```

3. 直接添加chrome仓库文件的方法

    下面两个操作与上面两步任选其一即可。
    - 直接执行以下脚本，添加google-chrome仓库文件并启用。
    
        ```
        cat << EOF > /etc/yum.repos.d/google-chrome.repo
        [google-chrome]
        name=google-chrome
        baseurl=http://dl.google.com/linux/chrome/rpm/stable/x86_64
        enabled=1
        gpgcheck=1
        gpgkey=https://dl.google.com/linux/linux_signing_key.pub
        EOF
        ```
    - 从网上下载 chrome 仓库文件
        ```shell
        cd /etc/yum.repos.d/
        sudo wget  http://repo.fdzh.org/chrome/google-chrome-mirrors.repo
        ```
4. 安装

    ```
    dnf install google-chrome-stable
    ```
    
    如果 centos 中不支持 dnf，换成 yum 即可

> 第1、2、4步也可使用 gnome-software 完成

# 中文输入法

> [fcitx vs ibus](http://einverne.github.io/post/2019/08/linux-input-method-fcitx-ibus.html)
> 
> [rime下载安装](https://rime.im/download/)
> 
> [安装Linux中文输入法并配置](https://juejin.im/post/5da1d534e51d457825210a8d)
> 
> **[Rime官方基础教程](https://github.com/rime/home/wiki/RimeWithSchemata)**

Linux 中的两种支持中文的输入法框架 fcitx 和 ibus。 
ibus 好像用的比较多，fedora 预装的就是 ibus，ibus-libpinyin 和 ibus-libzhuyin 是预装的中文输入法，用起来也还行，虽然没有 搜狗那么智能，但是用起来很稳定。

搜狗拼音 Linux 版的是基于 fcitx 框架的，之前装过一次，但是fcitx框架在fedora上用起来非常不稳定，输入比较卡而且经常崩溃。

rime 这个输法可以非常好支持中文繁简体的输入，尤其是繁体中文的准确度很高，还有奇奇怪怪种类繁多的拼音、字形输入，还可以自己 DIY ，总之是个相当强大的输入法。它的 Linux 版叫中州韵，Windows 版叫小狼毫，Mac 版叫鼠鬚管，Linux 版有 ibus-rime 和 fcitx-rime。之前装过一次 ibus-rime 用起来溜溜哒，强推。

执行以下命令安装 ibus-rime：

```
sudo dnf install ibus-rime
```

安裝完成后，到 `Settings -> Region & Language -> Input Sources -> Chinese ` 中添加输入法 ，***注意输入法不会立即出现在输入语言选项中***，我是在注销重新登录后(据查还可以用 `ibus restart` 命令)，才在 Input Sources Chinese 中找到 Chinese(Rime)。

第一次启用输入法的时候有点卡，刚启用的时候可能后台有一些初始化工作，我用的有时虚拟机本身性能就差，所以它卡了半天才能正常输入。

它默认的输入方案是“朙月拼音”，是繁体字，`ctrl + ~` 或 `F4` 可以调出设置输入方案的浮窗，选择`2 中/半/汉/。` -> `4 汉字->漢字` 或者，直接选择`4 朙月拼音·简化字`。

这个输入法的缺点也很明显：功能太复杂初学者上手太痛苦。我为了输`·`这个就折腾了一个多小时，查官方文档，翻用户配置文件的`~/.config/ibus/rime/build/default.yaml`，才找到要用`|`来输`·`，太痛苦啦~
