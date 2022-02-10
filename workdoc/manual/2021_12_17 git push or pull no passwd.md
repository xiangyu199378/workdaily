git 提交 不用输入用户名、密码的方法（GIT免密提交
1. 在项目所在位置打开git-bash， 在git bash交互环境输入命令：
- git config credential.helper store
- 不加参数： --global 只对这个仓库生效，并非全局设置 。
- 后续正常 push，第一次要输入账号密码，以后就不用了。
