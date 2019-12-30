# npm第一次发布自己的包

1. 在官网注册账号 https://www.npmjs.com/

2. 创建自己的包

   新建文件夹如 npm

   ```shell
   $ mkdir npm 
   $ cd npm
   # 
   $ npm init  
   $ touch index.js
   $ vi index.js 
   ```

   `index.js`

   ```javascript
   exports.hi=function () {
     return "hello world!";
   };
   ```

   

3. 发布

   登录账号

   ```shell
   $ npm login
   ```

   报错：

   ```shell
   Username: fackin
   Password:
   Email: (this IS public) xxxxxxxxx@163.com
   npm ERR! code E401
   npm ERR! 404 Unauthorized
   
   npm ERR! A complete log of this run can be found in:
   ```

   原因：使用了别的加速源

   ```shell
   $ cat ~/.npmrc
   $ vi ~/.npmrc
   ## 注释掉
   # registry=http://mvn3.qdingnet.com/nexus/repository/npm-group/
   ```

   或：

   ```shell
   $ nrm ls
   $ nrm use npm
   ```

   发布 `npm publish`

   ```shell
   $ npm publish
   # 报错
   npm ERR! publish Failed PUT 403
   npm ERR! code E403
   npm ERR! you must verify your email before publishing a new package: https://www.npmjs.com/email-edit : first-test-pkg
   ```

   原因：开通的账号没有通过邮箱验证

   邮箱验证后

   ```shell
   $ npm publish
   + first-test-pkg@1.0.0
   ```

4. 下载使用

   另一个文件夹中

   ```shell
   $ npm install first-test-pkg
   $ touch test.js
   $ vi test.js
   ```

   `test.js`

   ```javascript
   const test = require('first-test-pkg');
   console.log(test.hi());
   ```

   ```shell
   $ node test.js
   hello world!
   ```

   （完）

参考：

https://blog.csdn.net/zyg1515330502/article/details/81112653

https://www.jianshu.com/p/a1bb4c317c3e

https://blog.csdn.net/AinUser/article/details/88710249