# blockchain-explorer
SM-fabric

# 区块链浏览器本地启动

## 一、环境安装

需要的环境为：

- nodejs 8.11.x (Note that v9.x is not yet supported)
- PostgreSQL 9.5 or greater
- Jq [https://stedolan.github.io/jq/]

所以执行下面命令安装相应部分

1.1 安装Jq

```
sudo apt-get install jq
```

2.2 安装node，版本要求8.11.x，所以采用8.11.0版本

nvm国内镜像设置

```
export NVM_NODEJS_ORG_MIRROR=http://npm.taobao.org/mirrors/node
export NVM_IOJS_ORG_MIRROR=http://npm.taobao.org/mirrors/iojs
```

下载nvm脚本

```
cd ~
git clone https://gitee.com/steinven/nvm.git
sh ~/nvm/install.sh
```

退出当前窗口。

安装 node v8.11.0

```
nvm install 8.11.0
```

可以安装多个版本，使用use进行切换，如

```
nvm use 8.11.0
```

2.3 安装PostgreSQL 

```
sudo apt-get install postgresql
```

## 二、下载代码

- `git clone https://github.com/hyperledger/blockchain-explorer.git`.
- `cd blockchain-explorer`.
- `git checkout v0.3.7`

## 三、数据库启动

- `cd blockchain-explorer/app`

- 修改 explorerconfig.json 以更新 postgresql 属性

  - postgreSQL host, port, database, username, password details.
```
"postgreSQL": {

    "host": "127.0.0.1",
    "port": "5432",
    "database": "fabricexplorer",
    "username": "hppoc",
    "passwd": "password"
}
```

  配置数据库属性的另一种替代方法是使用环境变量，设置示例:

  - export DATABASE_HOST=127.0.0.1
  - export DATABASE_PORT=5432
  - export DATABASE_DATABASE=fabricexplorer
  - export DATABASE_USERNAME=hppoc
  - export DATABASE_PASSWD=password

  每次 git pull 后重要的重复（在某些情况下，您可能需要从 blockchain-explorer/app/persistence/fabric/postgreSQL 运行：`chmod -R 775 db/ ` 获取 db/ 目录应用权限）

运行创建数据库脚本

- `cd blockchain-explorer/app/persistence/fabric/postgreSQL/db`
- `./createdb.sh`

进入postgreSQL 查看是否建表

安装postgreSQL 后，系统会创建一个数据库超级用户 postgres，密码为空。

`sudo -i -u postgres`

上述命令切换至 postgres用户。

`psql`

`\l`查看数据库fabricexplorer是否建立

`\c fabricexplorer`切换至数据库fabricexplorer

`\d`查看所有表名，确保表已建立

## 四、fabric网络设置

根据自己启动的fabric网络设置文件`/blockchain-explorer/app/platform/fabric/config.json`

修改其中的fabricpath，为自己的fabric路径。like :```/home/za/go/src/github.com/hyperledger/fabric-sample/first-network/crypto-config ...```

"tlsEnable": false，
删除tls路径，grpcs --> grpc

## 五、建立Hyperledger Explorer

sudo apt-get install python g++

在另一个窗口，执行下述命令

```
cd blockchain-explorer
npm install
```

```
cd blockchain-explorer/app/test
npm install
npm run test
```

```
cd client/
npm install
npm test -- -u --coverage
```
test环节出现问题。修改`/blockchain-explorer/client/src/components/View/LandingPage.js`在17行添加`getBlockActivity: jest.fn(),`

```
npm run build
```


修改：
