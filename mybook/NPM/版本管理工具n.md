# node版本管理工具n

## 安装n

```shell
$ npm install n -g
```

## 使用n安装各版本node

```shell
$ sudo n lts     ## 安装最新版本
$ sudo n stable  ## 安装稳定版本
$ sudo n latest  ## 安装最新版本
$ sudo n 10.15.0 ## 安装指定版本
```

## 切换版本

```shell
$ n
o node/8.9.1
  node/10.15.0
```

## 删除版本

```shell
$ sudo n rm <版本号>
```

