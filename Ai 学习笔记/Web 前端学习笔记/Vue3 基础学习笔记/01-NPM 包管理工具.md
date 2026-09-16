  # 了解 NPM 包管理工具

&emsp;&emsp;NPM 全称 Node Package Manager，它是 <font color=red>**JavaScript 的包管理工具，并且是 Node.js 平台的默认包管理工具**</font>，通过 NPM 可以 <font color=red>**安装、共享、分发代码，管理项目依赖关系**</font>。

- 可以从 NPM 服务器下载别人编写的第三方包到本地使用
- 可以从 NPM 服务器下载并安装别人编写的命令行程序到本地使用
- 可将自己编写的包或命令行程序上传到 NPM 服务器供别人使用

&emsp;&emsp;可以把 NPM 理解为前端的 Maven ，<font color=red>**通过 NPM 可以很方便地安装与下载 JS 库、管理前端工程**</font>。当下版本的 Node.js 已经集成了 NPM 工具，所以必须首先在本机安装 Node 环境，安装完成后，可以通过下面的命令查看 Node 与 NPM 的版本：

```shell
$ node -v
v18.16.0

$ npm -v
9.5.1
```

# NPM 初始化项目

&emsp;&emsp;首先创建一个 <font color=brown>***npm-demo***</font> 的文件夹，通过命令提示符窗口进入到该文件夹，然后执行下面的命令进行初始化项目：

```shell
$ npm init
```

&emsp;&emsp;然后根据提示输入相关信息（<font color=red>**如果使用默认值，直接回车即可**</font>），最后会生成一个 <font color=brown>*__package.json__*</font> 文件（包的配置文件），之后也可以根据需要进行修改：

```json
{
  "name": "npm-demo", // 包名，其实就是项目名称（不能有大写字母）
  "version": "1.0.0", // 项目版本号
  "description": "first npm demo", // 项目描述
  "main": "index.js",  // 程序的主入口文件
  "scripts": {
    // 脚本命令组成的对象
    // 如果 test 测试环境，dev 开发环境，prod 生产环境
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "npm",
    "demo"
  ],
  "author": "andy", // {Array} 关键字，便于用户搜索到我们的项目
  "license": "ISC"  // 许可证 默认即可
}
```

> <font color=orange>**提示：**</font> 如果初始化项目的时候全部采用默认信息，则可以使用 <font color=green>***npm init -y***</font> 命令初始化项目，此时不会提示你手动输入信息。

# 配置淘宝镜像加速

&emsp;&emsp;NPM 默认下载依赖是从国外镜像地址（`https://registry.npmjs.org/`）中下载依赖，存在下载比较慢或者网络被限制等问题。通过配置国内镜像便可以解决问题：

```shell
# 查看当前使用的镜像地址
$ npm get registry

# 配置淘宝镜像地址
$ npm config set registry https://registry.npmmirror.com/

# 还原默认镜像地址
$ npm config set registry https://registry.npmjs.org/
```

# 安装模块

## 本地安装和全局安装

&emsp;&emsp;<font color=green>***npm install***</font> 命令用于安装某个模块，安装方式分为：<font color=red>**本地安装 和 全局安装**</font>：

+ <font color=orchid>**本地安装：**</font>将 JS 库安装在执行命令时所在的目录下，命令为 <font color=green>***npm install \<Module Name\>[@版本号]***</font>
+ <font color=orchid>**全局安装：**</font>将 JS 库安装到全局目录下，命令为 <font color=green>***npm install \<Module Name\>[@版本号] -g***</font>

> <font color=orange>**提示：**</font>如果在安装的时候出现下面的错误 <font color=red>**npm err! Error:connect ECONNREFUSED 127.0.0.1:8087**</font> ，可以使用 <font color=green>***npm config set proxy null***</font> 命令来解决。

<font color=orachid>**（一）本地安装**</font>

&emsp;&emsp;本地安装就是将 JS 库安装在当前目录下（如：本地安装 `jquery` 库）：

```shell
$ npm install jquery
```

> <font color=orange>**提示：**</font>安装过程中如果出现黄色的警告信息可以忽略。

&emsp;&emsp;安装成功后就会在当前目录下出现一个 <font color=brown>***node_modules***</font> 文件夹和 <font color=brown>***package-lock.json***</font> 文件：

+ <font color=orchid>**node_modules：**</font>该文件夹用于存放下载的 JS 库
+ <font color=orchid>**package-lock.json：**</font>该文件是执行 <font color=green>***npm install***</font> 命令的时候生成的一个文件，用来记录当前状态下实际安装的各个包的具体来源和版本号

&emsp;&emsp;这个时候重新打开 <font color=brown>***package.json***</font> 文件，就可以看到下载好的 `jquery.js` 已经添加到依赖列表中：

```json
{
  "name": "npm-demo",
  "version": "1.0.0",
  "description": "first npm demo",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "npm",
    "demo"
  ],
  "author": "andy",
  "license": "ISC",
  "dependencies": {
    "jquery": "^3.7.0"
  }
}
```

> <font color=orange>**关于模块的版本号表示方式：**</font>
>
> &emsp;&emsp;可以使用 <font color=green>***npm install jquery@2.2.0***</font> 命令下载指定版本的模块，关于版本号的说明如下：
>
> + <font color=orchid>**指定版本号（3.5.2）：**</font>只安装指定版本，遵循 `大版本.次要版本.小版本` 的格式规定
> + <font color=orchid>**～ + 指定版本号（～3.5.2）：**</font>安装 `3.5.x` 的最新版本（不低于 `3.5.2`），但是不安装 `3.6.x`，也就是说安装时不改变大版本号和次要版本号
> + <font color=orchid>**^ + 指定版本号（^3.5.2）：**</font>安装 `3.x.x` 的最新版本（不低于 `3.5.2`），但是不安装 `4.x.x`，也就是说安装的时候不改变大版本号。需要注意的是，如果大版本号为 0，则插入号的行为和波浪号相同，这是因为此时处于开发阶段，即使是次要版本号的变动，也可能带来程序的不兼容
> + <font color=orchid>**latest：**</font>安装最新版本

<font color=orachid>**（二）全局安装**</font>

&emsp;&emsp;全局安装会将库安装到全局目录下，可以使用 <font color=green>***npm root -g***</font> 命令查看当前的全局目录在哪里，也可以通过 <font color=green>***npm config set prefix "D:\npm"***</font> 命令来修改默认的全局目录。

&emsp;&emsp;可以使用下面的命令来进行全局安装模块（如：全局安装 `Vue` 脚手架）：

```shell
$ npm install @vue/cli -g
```

&emsp;&emsp;安装完成后，可以使用下面的命令来查看：

```shell
$ npm list -g
/usr/local/lib
├── @vue/cli@5.0.8
├── corepack@0.17.0
└── npm@9.5.1
```

## 生产环境安装

&emsp;&emsp;生产环境安装就是把模块的版本信息保存在 <font color=brown>***package.json***</font> 文件的 <font color=red>**dependencies**</font> 字段中，可以使用 <font color=green>***--save***</font> 或 <font color=green>***-S***</font> 参数：

```shell
$ npm install <Module Name> [--save|-S]
```

&emsp;&emsp;比如在生产环境中安装 `Vue` 模块：

```shell
$ npm install vue -S
```

&emsp;&emsp;下载完成后就可以在 <font color=brown>***package.json***</font> 文件的 <font color=red>**dependencies**</font> 字段中看到下载的模块信息：

```json
{
  "name": "npm-demo",
  "version": "1.0.0",
  "description": "first npm demo",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "npm",
    "demo"
  ],
  "author": "andy",
  "license": "ISC",
  "dependencies": {
    "vue": "^3.3.4"
  }
}
```

## 开发环境安装

&emsp;&emsp;开发环境安装就是把模块的版本信息保存在 <font color=brown>***package.json***</font> 文件的 <font color=red>**devDependencies**</font> 字段中，可以使用 <font color=green>***--save-dev***</font> 或 <font color=green>***-D***</font> 参数：

```shell
$ npm install <Module Name>[--save-dev|-D]
```

&emsp;&emsp;比如安装 `eslint` 模块（它是用来进行语法格式校验的，只要在开发环境依赖中即可）：

```shell
$ npm install eslint -D
```

&emsp;&emsp;下载完成后就可以在 <font color=brown>***package.json***</font> 文件的 <font color=red>**devDependencies**</font> 字段中看到下载的模块信息：

```json
{
  "name": "npm-demo",
  "version": "1.0.0",
  "description": "first npm demo",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "npm",
    "demo"
  ],
  "author": "andy",
  "license": "ISC",
  "dependencies": {
    "vue": "^3.3.4"
  },
  "devDependencies": {
    "eslint": "^8.44.0"
  }
}
```

## 批量下载模块

&emsp;&emsp;当从网上下载某些项目后，发现只有 <font color=brown>***package.json***</font> 文件，而没有 <font color=brown>***node_modules***</font> 文件夹，这时候需要通过命令下载 JS 库。首先使用命令提示符窗口进入到 <font color=brown>***package.json***</font> 所在的目录，然后执行 <font color=green>***npm install***</font> 命令。此时，NPM 会自动下载 <font color=brown>***package.json***</font> 中所依赖的 JS 库。

# 查看模块命令

<font color=orachid>**（一）查看本地已经安装的模块**</font>

&emsp;&emsp;可以在安装目录（<font color=brown>***node_modules***</font>）下查看包是否存在，也可以通过命令的方式查看：

```shell
# 查看本地安装的所有的模块
$ npm list

# 查看指定模板
$ npm list <Module Name>
```

<font color=orachid>**（二）查看远程版本**</font>

```shell
# 查看远程最新版本
$ npm view <Module Name> version

# 查看远程所有版本
$ npm view <Module Name> versions
```

# 卸载模块

```shell
# 卸载局部模块
$ npm uninstall <Module Name>

# 卸载全局模块
$ npm uninstall -g <Module Name>
```

