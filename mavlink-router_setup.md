# 前提条件
OS，その他ソフトウェアのバージョンは2022年6月24日時点で最新のものを使用する

# 使用するソフトウェアのインストール
ここでは以下のソフトウェアをインストールする．
- git
- meson
- ninja-buils
```
sudo apt install git
sudo apt install meson
sudo apt install ninja-build
```
pythonのmesonをインストールする.  
OSによっては`pip`がデフォルトではインストールされていないので手動でインストールする必要がある．
```
sudo apt-get -y install python3-pip
sudo pip3 install meson
```
以上で，mavlink-routerをインストールする準備が完了した．

# インストール
ソースコードをgithubからクローンし，サブモジュールもダウンロードする．その後，`meson`を用いてビルドの準備をした後，`ninja`を用いてビルド，インストールする．
```
git clone https://github.com/intel/mavlink-router.git
cd mavlink-router
git submodule update --init
meson setup build .
ninja -C build
sudo ninja -C build install
```
# 設定ファイルの作成
```
sudo mkdir /etc/mavlink-router
cd /etc/mavlink-router
sudo nano main.conf
```
コンフィグファイルの例
```conf:main.conf
[General]
   TcpServerPort=5760
   ReportStats=false
   MavlinkDialect=common

[UartEndpoint serial0]
   Device=/dev/serial0
   Baud=38400

[UdpEndpoint local]
   Mode=normal
   Address=127.0.0.1
   Port=14550
```

# ラズパイのハードウェアの設定
- フライとコントローラのシリアルをラズパイのシリアル１に接続する．
- ラズパイのシリアルポートを有効にする．
- ラズパイのシリアルを通したシェルへのアクセスを無効にする．

```
sudo raspi-config
> 3 Interface Option
> I6 Serial Port
> No
> Yes
> Ok
> Finish
```
`sudo raspi-config`により設定ツールが起動します．`> `がついている行は選択肢が表示されます．

# 実行
以下のコマンドにより実行できます．
```
mavlink-routerd
```

# 注意事項
本記事はmavlink routerを実行できることまでは動作確認しましたが，実際にFCに接続した動作確認はしていません．
