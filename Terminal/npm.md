```shell
sudo npm install -g npm
```
&emsp;`或`
```shell
npm install npm@latest -g
```
#### 安装指定版本
```shell
npm install -g npm@1.2.3
```

---

更新全局包：
```shell
npm update <name> -g
```

更新生产环境依赖包：
```shell
npm update <name> --save
```

更新开发环境依赖包：
```shell
npm update <name> --save-dev
```

---
## 依赖升级工具 npm-check
安装
```shell
npm install -g npm-check
```
使用
```shell
# 更新全局依赖
$ npm-check -gu
# 更新当前项目依赖
$ npm-check -u
```
> 修改 `.npmrc` 来修改命令行环境中 npm 的环境变量，开启高速（极速是阿里内部的 tnpm）依赖检查 & 安装模式。
使用npm命令（npm config set <key> <value> ）就能够自动创建.npmrc文件并且向文件内追加npm的配置项。比如，
```shell
npm config set registry https://registry.npm.taobao.org
```

## 查看安装过的包
```shell
npm list -g --depth 0
```
## 小技巧
* 当package.json中有某个模块的版本号时，你重新安装一遍，是不会自动更新到最新版本的，我们需要在json文件中，把该行版本号删除，你再安装，就会自动安装该模块的最新版本了
* npm i npm@latest -g更新npm到最新版本