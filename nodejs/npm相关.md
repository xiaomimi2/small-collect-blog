##npm 相关
1. npm init 初始化项目 npm init -y 默认直接创建完成。
2. npm install -h 查看可安装命令
3. npm i  xxx --save 保存写入dependencies，快捷命令 npm i -S
4. npm i  xxx --save-dev 写入devDependencies,快捷命令 npm i -D
5. npm i xxx --save --save-exact 安装同时将固定的一个版本写入dependencies,如果未指定版本就是最新的版本号
6. npm config set save-exact true ,这样每次npm i xxx --save的时候就会锁定依赖的版本号。*npm config set 命令将配置写到了~/.npmrc文件,运行npm config list查看*

