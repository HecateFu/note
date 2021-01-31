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
