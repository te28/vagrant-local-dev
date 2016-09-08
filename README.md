# vagrant + ansible ローカル開発環境構築

## 概要

ローカルのLAMP環境をvagrant + ansible(ansible_local)で作成します。
以下の環境が構築できます。

### 構築環境

* CentOS 6.7
* PHP 5.6
* MySQL 5.6
* Apache 2.2
* Ruby 2.3.1
* Nodejs 4.4

## 動作環境

以下をリンク先からインストールして下さい。

* [Virtual Box](https://www.virtualbox.org/)
* [Vagrant](https://www.virtualbox.org/)

### vagrant プラグイン

vagrantインストール後、以下コマンドでインストールして下さい。

```
vagrant plugin install [プラグイン名]
```

* [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest)
* [vagrant-hostmanager](https://github.com/devopsgroup-io/vagrant-hostmanager)


## 使用方法

### リポジトリからテンプレートを取得

任意のディレクトリに移動して、このテンプレートをリポジトリから取得してください。

```
$  ~/hoge
$ git clone https://github.com/te28/vagrant-local-dev
```

### 設定ファイルの編集

`Vagrantfile`を開き、下記の`ip`,`hostname`を任意の値に編集してください。

```
config.vm.network "private_network", ip: "192.168.33.XX"
```

```
config.vm.hostname = "xxx.local"
```

### vagrantの起動

以下コマンドを実行してください。

```
$ vagrant up
```

`hostname`で設定したアドレスでIt Works!と表示されればOKです。

### 設定を変更し、適用する場合

```
$ vagrant provision
```

## 構成ファイル

* Inventry:どのサーバーを管理するかを記述する(/.local)
* ansible.cfg:ansibleの設定を記述(./ansible.cfg)
* Playbook:どういった構成を作るかを記載(./xxxx.yaml)

```
.
├── README.md // 当ファイル
├── Vagrantfile // vagrantのプログラム
├── ansible // ansibleのタスク群
│   ├── ansible.cfg // ansible全般の設定を記述
│   ├── host_vars
│   ├── hosts // インベントリファイル どのサーバーを管理するのか記述
│   ├── roles
│   │   ├── common // 環境構築に必要なパッケージを設定
│   │   │   ├── handlers
│   │   │   │   └── main.yml
│   │   │   └── tasks
│   │   │       ├── ansible.yml
│   │   │       ├── main.yml
│   │   │       ├── utility.yml
│   │   │       └── yum_repos.yml
│   │   ├── httpd // apacheの設定
│   │   │   ├── handlers
│   │   │   │   └── main.yml
│   │   │   ├── tasks
│   │   │   │   ├── apache.yml
│   │   │   │   └── main.yml
│   │   │   └── templates
│   │   │        └── vhost.conf.j2
│   │   ├── mysql // mysqlの設定
│   │   │   ├── defaults
│   │   │   ├── files
│   │   │   ├── handlers
│   │   │   ├── meta
│   │   │   └── tasks
│   │   │   │   ├── main.yml
│   │   │   │   └── mysql.yml
│   │   ├── node // nodejsの設定
│   │   │   └── tasks
│   │   │   　   ├── main.yml
│   │   │   　   └── node.yml
│   │   ├── php // phpの設定
│   │   │   ├── handlers
│   │   │   │   └── main.yml
│   │   │   ├── tasks
│   │   │   │   ├── main.yml
│   │   │   │   └── php.yml
│   │   │   ├── templates
│   │   │   └── vars
│   │   ├── ruby // rubyの設定
│   │   │   ├── defaults
│   │   │   ├── files
│   │   │   │   └── bash_profile.custom
│   │   │   ├── handlers
│   │   │   │   └── main.yml
│   │   │   ├── tasks
│   │   │   │   ├── main.yml
│   │   │   │   └── ruby.yml
│   │   │   ├── templates
│   │   │   └── vars
│   │   │       └──main.yml
│   │   └── wordpress // wordpressの設定
│   │       ├── defaults
│   │       ├── files
│   │       ├── handlers
│   │       ├── tasks
│   │       │   ├── config.yml
│   │       │   ├── copy_and_edit_index_php.yml
│   │       │   ├── create_database.yml
│   │       │   ├── download.yml
│   │       │   ├── install.yml
│   │       │   ├── main.yml
│   │       │   ├── update.yml
│   │       │   ├── wordmove.yml
│   │       │   └── wpcli.yml
│   │       ├── templates
│   │       │   ├── movefile.j2
│   │       │   └── wp-config.php
│   │       └── vars
│   │           └── main.yml
│   └── site.yml
└── html // 仮想環境のドキュメントルート以下が同期されます。
```

## （参考）Ansibleとは

### python製のサーバーの構成管理ツール
<http://www.ansible.com/home>

### Vagrant + Ansible で開発環境を作るなら ansible_local プロビジョナがいい！
<http://blog.shin1x1.com/entry/ansible_local-provisioner-in-vagrant>

