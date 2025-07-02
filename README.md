# zama

# 1.node version確認
```
node -v

npm -v
```
偶数のversionにする →　v22.17.xなど

<https://nodejs.org/en/download>

# 2.リポジトリをclone
<https://github.com/zama-ai/fhevm-hardhat-template>
```
cd <your-preferred-location>
git clone <url-to-your-new-repo>
cd <your-new-repo-name>
```

# 3.hardhat
`npm install`
`npx hardhat vars set MNEMONIC`









# Movement-deploy

個人的な備忘録です。

Movementのテストネットがあり、興味があったのでdeployしてみました。

# 1.Metamaskの設定
- 手動でネットワークを追加
    - Network Name: Move-EVM
    - Network RPC: https://mevm.devnet.m1.movementlabs.xyz/v1
    - Network ID: 0x0150
    - Network Currency: MOV
<br>
- ID：0x150　画像で確認すると0150ではない。0150でやるとエラーになる。

# 2.faucet
以下のリンクよりアクセス。<br>
    <https://faucet.movementlabs.xyz/?network=testnet>
<br>M1 faucetにMove-EVMのウォレットアドレスを入力し、faucetを押す。<br>
<br>1 MOV貰えます。

# 3.hardhatとFractalを使用してM1へのコントラクトをデプロイする

## 1.プロジェクトディレクトリの作成する
```
mkdir hardhat-move-evm22
```

## 2.プロジェクトディレクトリへ移動
```
cd hardhat-move-evm
```

## 3.hardhatのプロジェクトを作成
 ```
 npx hardhat init
```
 
 今回は、JSを選択しました。

## 4.依存関係の確認

```
npm install --save-dev "hardhat@^2.19.0" "@nomicfoundation/hardhat-toolbox@^3.0.0"
```


## 5.dotenvのインストール
```
npm install dotenv
```


## 6..envファイルの作成
```
touch .env
```
中身は`PRIVATE_KEY=<your private key>` <br>
エディタはnanoを使用。


## 7.hardhat.config.jsの書き換え
```
require("@nomicfoundation/hardhat-toolbox");
require('dotenv').config();

module.exports = {
  defaultNetwork: "m1",
  networks: {
    hardhat: {
    },
    m1: {
      url: "https://mevm.devnet.m1.movementlabs.xyz/v1",
      accounts: [process.env.PRIVATE_KEY],
      chainId: 336,
      gasPrice: "auto",
    }
  },
  solidity: {
    version: "0.8.19",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200
      }
    }
  },
  paths: {
    sources: "./contracts",
    tests: "./test",
    cache: "./cache",
    artifacts: "./artifacts"
  }
}
```


## 8.実行
```
npx hardhat run scripts/deploy.js --network m1
```


# 4.実行結果
`Lock with 0.001ETH and unlock timestamp 1702990817 deployed to ********************************`


# 5.参考文献
<https://docs.movementlabs.xyz/developers/developer-tools/fractal/fractal-hardhat>

