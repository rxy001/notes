```shell
npm update <package> 升级依赖包版本

npm root -g 查看全局安装地址

npm repo <package> 浏览器端打开项目地址（GitHub）

npm home <package> 在浏览器端查看项目（项目主页）

npm config list  查看基本配置
npm config list -l  查看所有配置

npm config get prefix  获取全局安装的默认目录
npm config set prefix “directory”   设置全局安装的默认目录

npm config set registry https://registry.npm.taobao.org   淘宝镜像
npm config set registry http://registry.npmjs.org   npm
```

#### bin

当下载模块时，通过模块 package.json 的 bin 字段创建可执行文件。本地下载会在 node_modules 的.bin 文件夹里，全局下载则会加入到全局变量中。
具体参考 npm 文档：https://www.npmjs.com.cn/files/package.json/

模块的调试：

- 直接用相对路径安装

```
$ cd path/to/my-project
$ npm install path/to/my-utils
```

- npm link

```
$ cd path/to/my-project
$ npm link path/to/my-utils
```

如果这两种的目录不在一起，那还有一种方法：

```
$ # 先去到模块目录，把它 link 到全局
$ cd path/to/my-utils
$ npm link
$
$ # 再去项目目录通过包名来 link
$ cd path/to/my-project
$ npm link my-utils
```

该指令还可以用来调试 node cli 模块，譬如需要本地调试我们的 egg-init，可以这样

```
$ cd path/to/egg-init
$ npm link
$ # 此时全局的 egg-init 指令就已经指向你的本地开发目录了
$ egg-init # 即可
```

想去掉 link 也很简单：

```
$ npm unlink my-utils
```
