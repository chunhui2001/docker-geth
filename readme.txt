一、准备工作：

1、从官网下载以太坊go客户端
https://github.com/ethereum/go-ethereum/releases/

2、从官网下载以太坊钱包
https://github.com/ethereum/mist/releases/

3、创建 geth 初始化文件 hdgenesis.json

内容如下：

{
	# nonce就是一个64位随机数，用于挖矿，注意他和mixhash的设置需要满足以太坊的Yellow paper, 4.3.4. Block Header Validity, (44)章节所描述的条件。
    "nonce":"0x0000000000000042",
    # 与nonce配合用于挖矿，由上一个区块的一部分生成的hash。注意他和nonce的设置需要满足以太坊的Yellow paper, 4.3.4. Block Header Validity, (44)章节所描述的条件。
    "mixhash":"0x0000000000000000000000000000000000000000000000000000000000000000",
    # 设置当前区块的难度，如果难度过大，cpu挖矿就很难，这里设置较小难度
    "difficulty": "0x4000",
    # 用来预置账号以及账号的以太币数量，因为私有链挖矿比较容易，所以我们不需要预置有币的账号，需要的时候自己创建即可以。
    "alloc": {},
    # 矿工的账号，随便填
    "coinbase":"0x0000000000000000000000000000000000000000",
    # 设置创世块的时间戳
    "timestamp": "0x00",
    # 上一个区块的hash值，因为是创世块，所以这个值是0
    "parentHash":"0x0000000000000000000000000000000000000000000000000000000000000000",
    # 附加信息，随便填，可以填你的个性信息
    "extraData": "PICC GenesisBlock",
    # 该值设置对GAS的消耗总量限制，用来限制区块能包含的交易信息总和，因为我们是私有链，所以填最大。
    "gasLimit":"0xffffffff"

}

docker run -d --name ethereum-node -v /Users/alice/ethereum:/root -p 8545:8545 -p 30303:30303 ethereum/client-go

# 初始化创世区块
geth init ./genesis.json --datadir ./chain

4、创建主账户
geth console启动后，运行如下命令创建一个主账户（没有账户不允许挖矿），根据提示设置交易密码，创建成功后就可以使用console进行相关的单节点命令操作。

> personal.newAccount()

5、以太坊钱包
将以太坊钱包解压到指定安装目录，如d:\wallet，然后运行Ethereum-Wallet.exe 启动图形化的以太坊钱包，钱包默认连接本地的geth客户端，所以这时会显示的是私有网络(private-net)，点击Launch application启动钱包应用，进入钱包主界面即可看到账户信息，同时可以进行交易，创建智能合约等操作。

刚开始钱包是空的，没有任何以太，我们通过在控制台运行miner.start()命令开始挖矿赚钱，过一会就会发现钱包开始不断鼓起来了，超过500后运行miner.stop()命令停止挖矿。

点击ADD ACCOUNT创建一个新的账户ACCOUNT2.


用Postman来测试上面的HTTP-RPC服务了。
请求的json数据为: {"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":67}

{
    "jsonrpc": "2.0",
    "id": 67,
    "result": "Geth/v1.9.21-stable-0287d548/linux-amd64/go1.15.1"
}


同步模式--syncmode
同步模式有三种："fast", "full", or "light"
geth --syncmode "fast" --datadir DataDir --cache=512 console

"full":
获取区块的header
获取区块的body
从创始块开始校验每一个元素
下载所有区块数据信息

"fast":
获取区块的header
获取区块的body
在同步到当前块之前不处理任何事务，然后获得一个快照，像full节点一样进行后面的同步操作。沿着区块下载最近数据库中的交易，有可能丢失历史数据。
使用此模式时注意需要设置–cache，默认16M，我这里设置的是512M

"light":
仅获取当前状态。验证元素需要向full节点发起相应的请求。


console和attach
--------------------------
为了使用geth创建一个新帐户，我们必须首先在控制台模式下启动geth。

geth console 与 geth attach都可以打开一个JavaScript环境和节点进行交互，但是geth console会启动节点，geth attach是通过rpc或者ipc和已经启动的节点进行交互。在geth console里面可以使用所有模块的api，但是geth attach只能使用已经打开的模块的api，如果节点没有打开rpc geth attach甚至都不能连接上节点

geth console
geth attach

# Connecting to an Ethereum client with Java, Eclipse and Web3j
https://kauri.io/connecting-to-an-ethereum-client-with-java-eclipse/b9eb647c47a546bc95693acc0be72546/a



