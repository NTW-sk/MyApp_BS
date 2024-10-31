このプロジェクトは、AWSのCloudFormatioとAnsibleを使ってアプリケーション環境を構築するためのテンプレートを提供しています。

CloudFormation
▫️Application.yaml
	Application.yaml では、アプリケーションに関連するAWSリソースを定義しています。主に以下のリソースが含まれています:
		
		EC2 インスタンス: アプリケーションサーバーのインスタンスを定義します。
		セキュリティグループ: アプリケーションが使用するポートのアクセス許可を管理します。
		RDS インスタンス: アプリケーションで使用するデータベースの設定を定義します。

▫️Network.yaml
	Network.yaml では、ネットワーク関連のリソースを定義しています。
		
		VPC: このプロジェクト用の仮想ネットワークを作成します。
		サブネット: アプリケーションサーバー用とデータベース用に複数のサブネットを作成します。
		インターネットゲートウェイとルートテーブル: 外部アクセスを許可するための設定を行います。


▫️MyApp_Parent.yaml
	MyApp_Parent.yaml は親テンプレートで、Application.yamlとNetwork.ymlを一括で管理します。このテンプレートを使うことで、ネットワークとアプリケーションのリソースをまとめて作成できます。
		
		スタック呼び出し: NetworkスタックとApplicationスタックを順に呼び出し、それぞれのリソースを設定します。
		パラメータの管理: 共通するパラメータ（例: VPC ID、サブネットIDなど）を親テンプレートで一元管理します。

Ansible
▫️ディレクトリ構成
ansible
├── inventory
├── playbook.yml
└── roles
    ├── apache   
    ├── flask
    ├── proxy
    └── squid
        
▫️ディレクトリとファイルの説明
inventory: 管理対象のサーバー情報を記載したインベントリファイルです。このファイルで、どのサーバーに対してプレイブックを実行するかを指定します。
playbook.yml: Ansibleのメインのプレイブックです。複数のロールを呼び出し、サーバーに対する設定を順番に実行します。
roles: 
    apache: Apacheのインストールと設定を行います。必要なモジュールのインストールと設定ファイルの配置をしています。
    flask: Flaskアプリケーションのセットアップを行います。
    proxy: リバースプロキシとしてのApacheの設定を行います。
    squid: Squidプロキシのインストールと設定を行います。※appサーバはインターネットに接続できない構成のため、webサーバをproxyとしてミドルウェアのインストールを実施しました。

