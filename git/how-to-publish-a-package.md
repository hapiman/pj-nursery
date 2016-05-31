### 准备包名
检查所需安装的包是否已经存在，因为仓库的名称直接变成install的名称，[点我检查](https://www.npmjs.com/package/package-name-goes-here)

### 准备代码
1.`mkdir proj-name`创建项目的名称

2.在`github`上建立相应的仓库，并且`proj-name`连上仓库，`git init`和`git add remote origin xxx`

3.使用`npm init`初始化本地信息

4.创建相应的文件`index.js`,`README.md`等

### 发布需要当前发布者的用户信息
1.如果以前没有发布，没有发布帐户，直接建立一个信息的帐户,`npm adduser`
2.如果有帐户直接登陆,`npm login`
3.`npm publish`，直接使用该方式直接发布

###参考文档

1.[https://eladnava.com/publishing-your-first-package-to-npm/?utm_source=nodeweekly&utm_medium=email](https://eladnava.com/publishing-your-first-package-to-npm/?utm_source=nodeweekly&utm_medium=email)
