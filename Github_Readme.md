## 使用MsysGit
1. 注册github帐号
2. 下载http://code.google.com/p/msysgit/downloads/detail?name=Git-1.7.0.2-preview20100309.exe&can=2&q=
3. 安装git客户端，然后再About中生成SSH Pub Key
4. 把PubKey填入github
5. 在GIT-GUI中remote里加上git://git@github.com:xxxx/yyy.git 地址
6. 新建一个文件夹，GiT-GUI/Clone已有版本库，源路径填git://git@github.com:xxxx/yyy.git，即可取到本地
7. 提交到远程公共版本库：GIT GUI -> 远端 ->上传（最好先Add保存远端库Url）
8. 如果需要合并，则提交可能不成功，此时，需要GIT-GUI->合并->本地合并

## 使用TortoiseGit
1. 安装MsysGit
2. 安装TortoiseGit，在setting中设置MsysGit安装位置c:\program files\git\bin
3. 配置ssh-key，运行TortoiseGit的PuttyGen程序，Generate 密钥（需要把公钥放到github服务器上），或者Load用MsysGit生成的私钥（默认在~/.ssh/id_rsa，即c:\document and settings\xxx\.ssh目录下)，然后Save Private Key，
并在TortoiseGit settings的 git->Remote设置中加入的Remote站点中填入.ppk文件

## 建立本地仓库
1. 新建一个目录,例如 h:\code.git，
2. 使用msysgit的git bash然后输入 git init --bare,则本地仓库建立完备，
3. 跟服务器仓库一样，访问时直接用 h:\code.git 作为url 访问即可

