<!--toc=setup_server-->

# Dockerをインストールする

CMSの実行環境としては Docker が推奨されています。

> この章では、Dockerの環境構築に関して解説します。すでにDocker環境が設定されている場合は、本章を読み飛ばしてください。

## なぜDockerなのか？

Dockerはアプリケーションの実行に必要なプログラムやライブラリ、各種設定ファイルなどをパッケージにし、隔離して実行する仕掛けです。このパッケージを**コンテナ**と呼びます。

[[PRODUCTNAME]] CMSもこのコンテナで提供されています。

パッケージにされたコンテナは、Docker環境下なら各種サーバーで動かすことができ、隔離された環境を構築するため、セキュリティも向上し、個別の保守・運用管理ができます。


## リポジトリを使用してインストール

Docker エンジンを新しいホスト マシンに初めてインストールする前に、Docker リポジトリを設定する必要があります。その後、リポジトリから Docker をインストールおよび更新できます。

### リポジトリをセットアップする

1. パッケージ インデックスを更新し、aptパッケージをインストールしaptて、HTTPS 経由でリポジトリを使用できるようにします。

```
$ sudo apt-get update

$ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

2. Docker の公式 GPG キーを追加します。

```
$ sudo mkdir -p /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

3. 次のコマンドを使用して、リポジトリをセットアップします。

```
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Docker エンジンをインストールする

aptパッケージ インデックスを更新し、Docker Engine、containerd、および Docker Composeの最新バージョンをインストールします。

```
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
### インストールの確認

イメージを実行して、Docker エンジンが正しくインストールされていることを確認しhello-world ます。

```
 sudo service docker start
 sudo docker run hello-world

```

hello-worldを実行すると、正しくDocker エンジンがインストールされていれば、以下の内容が表示されます。

```
$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:62af9efd515a25f84961b70f973a798d2eca956b1b2b026d0a4a63a3b0b6a3f2
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### UbuntuユーザーでDockerを利用可能にする

Ubuntuのデフォルトでは、rootユーザーしかdockerを利用できません。都度rootユーザーに切り替えるのは面倒ですので、次のコマンドを実行して、一般ユーザーからもdockerを利用できるように設定しましょう。

```
$ sudo gpasswd -a ubuntu docker
```
