### brew常用命令

```shell
brew outdate           # 列出过时的包
brew install name      # 安装源码
brew info svn          # 显示软件的各种信息（包括版本、源码地址、依赖等等）
brew uninstall name    # 卸载软件
brew search name       # 搜索brew 支持的软件（支持模糊搜索）
brew list              # 列出本机通过brew安装的所有软件
brew update            # brew自身更新
brew upgrade name      # 更新安装过的软件(如果不加软件名，就更新所有可以更新的软件)
brew cleanup           # 清除下载的缓存
```
---

### 推荐 brew install cask

> `brew cask` 和 `brew` 的区别在于 `brew` 主要用来下载一些不带界面的命令行下的工具
 `brew cask` 主要用来下载一些图形化界面的应用软件，下载好后会自动安装，并能在mac中直接运行使用
 brew cask reinstall [name]

---

### brew cask常用命令

```shell
brew cask search            # 列出所有可以被安装的软件
brew cask search name       # 查找所有和 name相关的应用
brew cask install name      # 下载安装软件
brew cask uninstall name    # 卸载软件
brew cask info app          # 列出应用的信息
brew cask list              # 列出本机安装过的软件列表
brew cask cleanup           # 清除下载的缓存以及各种链接信息
brew cask uninstall name && brew cask install name ＃更新程序 （目前homebrew-cask 并没有命令直接更新已安装的软件，软件更新主要是通过软件自身的完成更新）
```

---

### 软件查询
```shell
 brew search /libreoffice\*/
 brew cask search /libreoffice\*/
```

---
### 更新包 (formula)

更新之前，用 `brew outdated` 查看哪些包可以更新。
```shell
brew outdated
```
然后就可以用 `brew upgrade` 去更新了。Homebrew 会安装新版本的包，但旧版本仍然会保留。
```shell
brew upgrade             # 更新所有的包
brew upgrade $FORMULA    # 更新指定的包
```
### 清理旧版本
一般情况下，新版本安装了，旧版本就不需要了。我会用 `brew cleanup` 清理旧版本和缓存文件。Homebrew 只会清除比当前安装的包更老的版本，所以不用担心有些包没更新但被删了。
```shell
brew cleanup             # 清理所有包的旧版本
brew cleanup $FORMULA    # 清理指定包的旧版本
brew cleanup -n          # 查看可清理的旧版本包，不执行实际操作
```
### 锁定不想更新的包
如果经常更新的话，`brew update` 一次更新所有的包是非常方便的。但我们有时候会担心自动升级把一些不希望更新的包更新了。数据库就属于这一类，尤其是 PostgreSQL 跨 minor 版本升级都要迁移数据库的。我们更希望找个时间单独处理它。这时可用 `brew pin` 去锁定这个包，然后 `brew update` 就会略过它了
```shell
brew pin $FORMULA      # 锁定某个包
brew unpin $FORMULA    # 取消锁定
```
### 其他几个常用命令
`brew info` 可以查看包的相关信息，最有用的应该是包依赖和相应的命令。比如 Nginx 会提醒你怎么加 launchctl ，PostgreSQL 会告诉你如何迁移数据库。这些信息会在包安装完成后自动显示，如果忘了的话可以用这个命令很方便地查看。
```shell
brew info $FORMULA    # 显示某个包的信息
brew info             # 显示安装了包数量，文件数量，和总占用空间
```
`brew deps` 可以显示包的依赖关系，我常用它来查看已安装的包的依赖，然后判断哪些包是可以安全删除的。
```shell
brew deps --installed --tree # 查看已安装的包的依赖，树形显示
```
输出如下：
```shell
elixir (required dependencies)
└── :erlang

wxmac (required dependencies)
├── jpeg
├── libpng
│   └── xz
└── libtiff
    └── jpeg
```