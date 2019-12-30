

# npm模块管理器

搬砖：https://www.jianshu.com/p/dee4a84e5961

[http://javascript.ruanyifeng.com/nodejs/npm.html](http://javascript.ruanyifeng.com/nodejs/npm.html)

## 1. what？npm是什么

npm是Nodejs官方提供的包管理工具，用于Nodejs包的发布、传播、依赖控制。npm提供了命令行工具，可以方便地下载、安装、升级、删除包，也可以作为开发者发布并维护包。

## 2. why？为什么使用npm

npm是随同Nodejs一起安装的包管理工具，能解决Nodejs代码部署上的很多问题，常见场景如下：

- 允许用户从npm服务器下载别人编写的第三方包到本地使用。
- 允许用户从npm服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到npm服务器供别人使用。

npm的背后，是基于couchdb的一个数据库，详细记录了每个包的信息，包括作者、版本、依赖、授权信息等。它的一个重要作用是，将开发者从繁琐的包管理工作（版本、依赖等）中解救出来，更加专注于功能的开发。

## 3. how？如何使用npm

### 安装

npm不需要单独安装。在安装Node的时候，会连带一起安装npm。但是，Node附带的npm可能不是最新版本，最后用下面的命令，更新到最新版本。

```shell
$ sudo npm install npm@latest -g
```

如果是Window 系统使用以下命令即可：

```shell
npm install npm -g
```

也就是使用npm安装自己。之所以可以这样，是因为npm本身与Node的其他模块没有区别。然后，运行下面的命令，查看各种信息。

```shell
# 查看npm命令列表
$ npm help

# 查看各个命令的简单用法
$ npm -l
# 查看 npm 的版本
$ npm -v

# 查看npm的配置
$ npm config list -l 
```



### npm init

`npm init`用来初始化生成一个新的`package.json`文件。它会向用户提问一系列问题，如果你觉得不用修改默认配置，一路回车就可以。

如果使用了-f（代表force）、-y（代表yes），则跳过提问阶段，直接生成一个新的`package.json`文件。

```shell
$ npm init -y
```

### npm set

`npm set`用来设置环境变量

```shell
$ npm set init-author-name 'Your name'
$ npm set init-author-email 'Your email'
$ npm set init-author-url 'http://yoururl.com'
$ npm set init-license 'MIT'
```

上面命令等于为`npm init`设置了默认值，以后执行`npm init`的时候，package.json的作者姓名、邮件、主页、许可证字段就会自动写入预设的值。这些信息会存放在用户主目录的`~/.npmrc`文件，使得用户不用每个项目都输入。如果某个项目有不同的设置，可以针对该项目运行`npm config`。

```shell
$ npm set save-exact true
```

上面命令设置加入模块时，`package.json`将记录模块的确切版本，而不是一个可选的版本范围。

### npm config

```shell
$ npm config set prefix $dir
```

上面的命令将指令的`$dir`目录，设为模块的全局安装目录。如果当前有这个目录的写权限，那么运行`npm install`的时候，就不再需要`sudo`命令授权了。

```shell
$ npm config set save-prefix ~
```

上面的命令使得`npm install —save`和`npm install --save-dev`安装新模块时，允许的版本范围从克拉符号（`^`）改成波浪号（`~`），即从允许小版本升级，变成只允许补丁包的升级。

```shell
$ npm config set init.author.name $name
$ npm config set init.author.email $email
```

上面命令行指定使用`npm init`时，生成的`package.json`文件的字段默认值。

### npm info

`npm info`命令可以查看每个模块的具体信息。比如，查看underscore模块的信息。

```shell
$ npm info underscore
```

上面命令返回一个JavaScript对象，包含了underscore模块的详细信息。这个对象的每个成员，都可以直接从info命令查询。

```shell
$ npm info underscore description
$ npm info underscore version
$ npm info underscore homepage
```

### npm search

`npm search`命令用于搜索npm仓库，它后面可以跟字符串，也可以跟正则表达式。

```shell
$ npm search <搜索词>
```

### npm list

`npm list`命令以树形结构列出当前项目安装的所有模块，以及它们依赖的模块。

```shell
$ npm list
```

加上global参数，会列出全局安装的模块。

```shell
$ npm list -global
```

`npm list`命令也可以列出单个模块。

```shell
$ npm list underscore 
```



### npm install

#### 基本用法

Node模块采用`npm install`命令安装。

每个模块可以"全局安装"，也可以"本地安装"。"全局安装"指的是将一个模块安装到系统目录中，各个项目都可以调用。一般来说，全局安装只适用于工具模块，比如`eslint`和`gulp`。"本地安装"指的是将一个模块下载到当前项目的`node_modules`子目录，然后只有在项目目录之中，才能调用这个模块。

```shell
# 本地安装
$ npm install <package name>

#全局安装
$ sudo npm install -global <package name>
$ sudo npm install -g <package name>
```

`npm install`也支持直接输入Github代码库地址。

```shell
$ npm install git://github.com/package/path.git
$ npm install git://github.com/package/path.git#0.1.1
```



安装之前，`npm install` 会先检查，`node_modules`目录之中是否已经存在指定模块。如果存在，就不再重新安装了，即使远程仓库已经有了一个新版本，也是如此。

如果你希望，一个模块不管是否安装过，npm都要强制重新安装，可以使用-f或—force参数。

```shell
$ npm install <packageName> --force
```

如果你希望，所有模块都要强制重新安装，那就删除`node_modules`目录，重新执行`npm install`。

```shell
$ rm -rf node_modules
$ npm install
```



#### 安装不同版本

install命令总是安装模块的最新版本，如果要安装模块的特定版本，可以在模块名后面加上`@`和版本号。

```shell
$ npm install sax@latest
$ npm install sax@0.1.1
$ npm install sax@">=0.1.0 <0.2.0"
```

Install 命令可以使用不同参数，指定所安装的模块属于哪一种性质的依赖关系，即出现在`packages.json`文件的那一项中。

> `--save`：模块名将被添加到dependencies，可以简化为参数-S。
>
> `--save-dev`：模块名将被添加到devDependencies，可以简化为参数-D。

```shell
$ npm install sax --save
$ npm install node-tap --save-dev
# 或者
$ npm install sax -S
$ npm install node-tap -D
```

##### dependencies依赖

这个可以说是我们npm核心一项内容，依赖管理，这个对象里面的内容就是我们这个项目所依赖的js模块包。下面这段代码表示依赖了`markdown-it`这个包，版本是`^8.1.0`，代表最小依赖版本是`8.1.0`，如果这个包有更新，那么当我们使用`npm install`命令的时候，`npm`会帮我们下载最新的包。当别人引用我们这个包的时候，包内的依赖包也会被下载下来。

```javascript
"dependencies": {
    "markdown-it": "^8.1.0"
}
```

##### devDevpendencies开发依赖

在我们开发的时候会用到一些包，只是在开发环境需要用到，但是在别人引用我们包的时候，不会用到这些内容，放在`devDenpendencies`的包，在别人引用的时候不会被`npm`下载。

```javascript
"devDependencies": {
    "autoprefixer": "^6.4.0",0",
    "babel-preset-es2015": "^6.0.0",
    "babel-preset-stage-2": "^6.0.0",
    "babel-register": "^6.0.0",
    "webpack": "^1.13.2",
    "webpack-dev-middleware": "^1.8.3",
    "webpack-hot-middleware": "^2.12.2",
    "webpack-merge": "^0.14.1",
    "highlightjs": "^9.8.0"
}
```

当你有了一个完整的`package.json`文件的时候，就可以让人一眼看出来，这个模块的基本信息，和这个模块所需要的依赖的包。我们可以通过`npm install`就可以很方便的下载好这个模块所需要的包。

`nom install`默认会安装`dependencies`字段和`devDenpendencies`字段中的所有模块，如果使用`--production`参数，可以只安装`denpendencies`字段的模块。

```shell
$ npm install --production
# 或者
$ NODE_ENV=production npm install
```

一旦安装了某个模块，就可以在代码中用`require`命令加载这个模块。

```javascript
var backbone = require('backbone')
console.log(backbone.VERSION)
```

### 避免系统权限（未）

默认情况下，Npm全局模块都安装在系统目录（比如`/usr/local/lib/`），普通用户没有写入权限，需要用到`sudo`命令。这不是很方便，我们可以在没有权限的情况下，安装全局模块。

首先，在主目录下新建配置文件`.npmrc`，然后在该文件中将`prefix`变量定义到主目录下。

```bash
prefix=/home/yourUsername/npm
```

然后在主目录下新建`npm`子目录。

```shell
$ mkdir ~/npm
```

此后，全局安装的模块都会安装在这个子目录中，Npm也会到`~/npm/bin`目录去寻找命令。

最后，将这个路径在`.bash_profile`文件（或`.bashrc`文件）中加入PATH变量。

```bash
export PATH=~/npm/bin:$PATH
```

### npm update

更新本地安装的模块。

```shell
# 升级当前项目的指定模块
$ npm update [package name]

# 升级全局安装的模块
$ npm update -global [package name]
```

它会先到远程仓库查询最新版本，然后查询本地版本。如果本地版本不存在，或者远程版本较新，就会安装。

使用`-S`或`--save`参数，可以在安装的时候更新`package.json`里面模块的版本号。

```json
// 更新之前的package.json
dependencies: {
  dep1: "^1.1.1"
}

// 更新之后的package.json
dependencies: {
  dep1: "^1.2.2"
}
```

**注意**，从npm v2.6.1开始，`npm update`只更新顶层模块，而不更新依赖的依赖，以前版本是递归更新的。如果想取到老版本的效果，要使用下面的命令。

```shell
$ npm --depth 9999 update
```

### npm uninstall

卸载已安装的模块。

```shell
$ npm uninstall [package name]

# 卸载全局模块
$ npm uninstall [package name] -global
```



### npm run

npm 不仅可以用于模块管理，还可以用于执行脚本。`package.json`文件有一个`scripts`字段，可以用于指定脚本命令，供`npm`直接调用。

`package.json`

```json
{
  "name": "myproject",
  "devDependencies": {
    "jshint": "latest",
    "browserify": "latest",
    "mocha": "latest"
  },
  "scripts": {
    "lint": "jshint **.js",
    "test": "mocha test/"
  }
}
```

#### scripts脚本

顾名思义，就是一些脚本代码，可以通过`npm run script-key`来调用，例如在这个`package.json`的文件夹下使用`npm run dev`就相当于运行了`node build/dev-server.js`这一段代码。使用scripts的目的就是为了把一些要执行的代码合并到一起，使用`npm run`来快速的运行，方便省事。

`npm run`是`npm run-script`的缩写，一般都使用前者，但是后者可以更好的反应这个命令的本质。

```json
// 脚本
"scripts": {
    "dev": "node build/dev-server.js",
    "build": "node build/build.js",
    "docs": "node build/docs.js",
    "build-docs": "npm run docs & git checkout gh-pages & xcopy /sy dist\\* . & git add . & git commit -m 'auto-pages' & git push & git checkout master",
    "build-publish": "rmdir /S /Q lib & npm run build &git add . & git commit -m auto-build & npm version patch & npm publish & git push",
    "lint": "eslint --ext .js,.vue src"
}
```

`npm run`命令会自动在环境变量`$PATH`添加`node_modules/.bin`目录，所以`scripts`字段里面调用命令时不用加上路径，这就避免了全局安装NPM模块。

`npm run`会创建一个Shell，执行指定的命令，并临时将`node_modules/.bin`加入PATH变量，这意味着本地模块可以直接执行。

举例来说，你执行ESLint的安装命令。

```shell
$ npm i eslint --save-dev
```

运行上面的命令以后，会产生两个结果。首先，ESLint被安装到当前目录的`node_modules`子目录；其次，`node_modules/.bin`目录会生成一个符号链接`node_nodules/.bin/eslint`，指向ESLint模块的可执行脚本。

然后，就可以在`package.json`的`script`属性里面，不带路径的引用`eslint`这个脚本。

```json
{
  "name": "Test Project",
  "devDependencies": {
    "eslint": "^1.10.3"
  },
  "scripts": {
    "lint": "eslint ."
  }
}
```

等到运行`npm run lint`的时候，它会自动执行`./node_modules/.bin/eslint .`。

`npm run`如果不加任何参数，直接运行，会列出`package.json`里面所有可以执行的脚本命令。

```shell
$ npm run
Lifecycle scripts included in npm:
  test
    echo "Error: no test specified" && exit 1
```



Npm内置了两个命令简写，`npm test`等同于执行`npm run test`，`npm start`等同于执行`npm run start`。

如果希望一个操作的输出，是另一个操作的输入，可以借用Linux系统的管道命令，将两个操作连在一起。

```json
"build-js": "browserify browser/main.js | uglifyjs -mc > static/bundle.js"
```

但是，更方便的写法是引用其他`npm run`命令

```json
"build": "npm run build-js && npm run build-css"
```

上面的写法是先运行`npm run build-js`，然后再运行`npm run build-css`，两个命令中间用`&&`连接。如果希望两个命令同时平行执行，它们中间可以用`&`连接。

下面是一个流操作的例子。

```json
"devDependencies": {
  "autoprefixer": "latest",
  "cssmin": "latest"
},

"scripts": {
  "build:css": "autoprefixer -b 'last 2 versions' < assets/styles/main.css | cssmin > dist/main.css"
}
```



写在`scripts`属性中的命令，也可以在`node_modules/.bin`目录中直接写成bash脚本。下面是一个bash脚本。下面是一个bash脚本。

```bash
#!/bin/bash

cd site/main
browserify browser/main.js | uglifyjs -mc > static/bundle.js
```

假定上面的脚本文件名为build.sh，并且权限为可执行，就可以在scripts属性中引用该文件。

```json
"build-js": "bin/build.sh"
```

#### 参数

`npm run`命令还可以添加参数。

```json
"scripts": {
  "test": "mocha test/"
}
```

上面代码指定`npm test`，实际运行`mocha test/`。如果要通过`npm test`命令，





https://www.jianshu.com/p/dee4a84e5961

http://javascript.ruanyifeng.com/nodejs/npm.html